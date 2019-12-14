#include <iostream>
#include <conio.h>
#include <string.h>
#include <stdlib.h>
#define MAXPATIENTS 100
#define WINDOWS 1

using namespace std;
void clrscr() {
  #ifdef WINDOWS
  system("cls");
  #endif
  #ifdef LINUX
  system("clear");
  #endif
}

struct patient
{
	char FirstName[50];
	char LastName[50];
	char ID[20];
};
class queue
{
public:
	queue(void);
	int AddPatientAtEnd(patient p);
	int AddPatientAtBeginning(patient p);
	patient GetNextPatient(void);
	int RemoveDeadPatient(patient *p);
	void OutputList(void);
	char DepartmentName[50];
private:
	int NumberOfPatients;
	patient List[MAXPATIENTS];
};
queue::queue()
{
	NumberOfPatients = 0;
}
int queue::AddPatientAtEnd(patient p)
{
	if (NumberOfPatients >= MAXPATIENTS)
	{
		return 0;
	}
	else
		List[NumberOfPatients] = p;
	NumberOfPatients++;
	return 1;
}
int queue::AddPatientAtBeginning(patient p)
{
	int i;
	if (NumberOfPatients >= MAXPATIENTS)
	{
		return 0;
	}
	for (i = NumberOfPatients - 1; i >= 0; i--)
	{
		List[i + 1] = List[i];
	}
	List[0] = p;
	NumberOfPatients++;
	return 1;
}
patient queue::GetNextPatient(void)
{
	int i; patient p;
	if (NumberOfPatients == 0)
	{
		strcpy(p.ID, "");
		return p;
	}
	p = List[0];
	NumberOfPatients--;
	for (i = 0; i < NumberOfPatients; i++)
	{
		List[i] = List[i + 1];
	}
	return p;
}
int queue::RemoveDeadPatient(patient *p)
{
	int i, j, found = 0;
	for (i = 0; i < NumberOfPatients; i++)
	{
		if (stricmp(List[i].ID, p->ID) == 0)
		{
			*p = List[i]; found = 1;
			NumberOfPatients;
			for (j = i; j < NumberOfPatients; j++)
			{
				List[j] = List[j + 1];
			}
		}
	}
	return found;
}
void queue::OutputList(void)
{
	int i;
	if (NumberOfPatients == 0)
	{
		cout << "Sira bos" << endl;
	}
	else
	{
		for (i = 0; i < NumberOfPatients; i++)
		{
			cout << " " << List[i].FirstName;
			cout << " " << List[i].LastName;
			cout << " " << List[i].ID; cout <<";" << endl;
		}
	}
}
patient InputPatient(void)
{
	patient p;
	cout << "Lutfen yeni hasta adi icin veri girin: " << endl;
	cin.getline(p.FirstName, sizeof(p.FirstName));
	cout << "Soy Isim : " << endl;
	cin.getline(p.LastName, sizeof(p.LastName));
	cout << "T.C. Kimlik numaraniz:" << endl;
	cin.getline(p.ID, sizeof(p.ID));
	if (p.FirstName[0] == 0 || p.LastName[0] == 0 || p.ID[0] == 0)
	{
		strcpy(p.ID, "");
		cout << "Error: Veriler gecerli degil, islem iptal edildi." << endl;
		getch();
	}
	return p;
}
void OutputPatient(patient*p)
{
	if (p == NULL || p->ID[0] == 0)
	{
		cout << "Hasta yok" << endl;
		return;
	}
	else
		cout << "Hasta Verileri:" << endl;
	cout << " Isim :" << p->FirstName << endl;
	cout << "Soyisim:" << p->LastName << endl;
	cout << "T.C. Kimlik numaraniz:" << p->ID << endl;
}
int ReadNumber()
{
	char buffer[20];
	cin.getline(buffer, sizeof(buffer));
	return atoi(buffer);
}
void DepartmentMenu(queue * q)
{
	int choice = 0, success; patient p;
	while (choice != 6)
	{
		clrscr();
		cout << "__________________________________________________________________________________________" << endl;
		cout << "\t""                         Departmana Hos Geldiniz:" << q->DepartmentName << endl;
		cout << "____________________________________________________________________________________________" << endl;
		cout << "Lutfen seciminizi girin : " << endl;
		cout << "1: Normal hasta " << endl;
		cout << "2: Kritik hastalik hastasi ekle" << endl;
		cout << "3: Operasyon icin hastayi cikarmak " << endl;
		cout << "4: Olu hasta kaydi silme" << endl;
		cout << "5: Liste sirasi" << endl;
		cout << "6: Departmani degistir veya cikis yap" << endl;
		choice = ReadNumber();
		switch (choice)
		{
		case 1:
			p = InputPatient();
			if (p.ID[0])
			{
				success = q->AddPatientAtEnd(p);
				clrscr();
				if (success)
				{
					cout << "Hasta ekle:" << endl;
				}
				else
				{
					cout << "Error:Sira dolu.Hasta eklenemiyor:" << endl;
				}
				OutputPatient(&p);
				cout << "Herhangi bir tusa basin" << endl;
				getch();
			}
			break;
		case 2:
			p = InputPatient();
			if (p.ID[0])
			{
				success = q->AddPatientAtBeginning(p);
				clrscr();
				if (success)
				{
					cout << "Hasta ekleme:" << endl;
				}
				else
				{
					cout << "Error:Sira dolu.Hasta ekleyemiyor:" << endl;
				}
				OutputPatient(&p);
				cout << "Herhangi bir tusa basin." << endl;
				getch();
			}
			break;
		case 3:
			p = q->GetNextPatient();
			clrscr();
			if (p.ID[0])
			{
				cout << "Ameliyat olacak hasta:" << endl;
				OutputPatient(&p);
			}
			else
			{
				cout << "Ameliyat olacak hasta yok." << endl;
			}
			cout << "Herhangi bir tusa basiniz." << endl;
			getch();
			break;
		case 4:
			p = InputPatient();
			if (p.ID[0])
			{
				success = q->RemoveDeadPatient(&p);
				clrscr();
				if (success)
				{
					cout << "Hasta cikarildi." << endl;
				}
				else
				{
					cout << "Error:Hasta bulunamadi." << endl;
				}
				OutputPatient(&p);
				cout << "Herhangi bir tusa basiniz." << endl;
				getch();
			}
			break;
		case 5:
			clrscr();
			q->OutputList();
			cout << "   " << endl;
			cout << "Herhangi bir tusa basiniz" << endl;
			getch();
			break;
		}
	}
}
int main()
{
	int i, MenuChoice = 0;
	queue departments[3];
	strcpy(departments[0].DepartmentName, "Heart clinic");
	strcpy(departments[1].DepartmentName, "Lung clinic");
	strcpy(departments[2].DepartmentName, "Plastic surgery");
	while (MenuChoice != 4)
	{
		clrscr();
		cout << "___________________________________________________________________" << endl;
		cout << "\t""              SEHIR HASTANESINE HOSGELDINIZ                  " << endl;
		cout << "___________________________________________________________________" << endl;
		cout << "Lutfen seciminizi giriniz:" << endl;
		for (i = 0; i < 3; i++)
		{
			cout << "   " << (i + 1) << ":   " << departments[i].DepartmentName << endl;
		}
		cout << "   4:   Cikis" << endl;
		MenuChoice = ReadNumber();
		if (MenuChoice >= 1 && MenuChoice <= 3)
		{
			DepartmentMenu(departments + (MenuChoice - 1));
		}
	}
}
