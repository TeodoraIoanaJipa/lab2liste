# lab2liste
#include <iostream>

using namespace std;

struct nod{
    int info;
    nod *next;
};

nod *prim,*ultim;

void adaug_final(int x)
{
    nod *aux;
    if(prim==NULL)
    {
        prim=new nod;
        prim->info=x;
        prim->next=NULL;
        ultim=prim;
    }
    else
    {
        aux=new nod;
        aux->info=x;
        aux->next=NULL;
        ultim->next=aux;
        ultim=aux;
    }

}

void adaug_inceput(int x)
{
    nod *aux;
    if(prim==NULL)
    {
        prim=new nod;
        prim->info=x;
        prim->next=NULL;
        ultim=prim;
    }
    else
    {
       aux=new nod;
       aux->info=x;
       aux->next=prim;
       prim=aux;
    }
}

int numar_noduri()
{
    int nr=0;
    nod *p;
    while (p!=NULL)
    {
        nr++;
        p=p->next;
    }
    return nr;
}


void adaug_interior(int x, int poz)
{
  int i;
  int nr=numar_noduri();
  nod *aux, *p=prim;

  if(prim==NULL)
  {
      adaug_inceput(x);
      return;
  }

  if(poz==nr)
  {
      adaug_final(x);
      return;
  }

  if(poz==1)
  {
      adaug_inceput(x);
      return;
  }

  aux=new nod;
  aux->info=x;
  for(i=1;i<poz;i++)
    p=p->next;
  aux->next=p->next;
  p->next=aux;

}


void afisare()
{
    nod *aux=prim;
    while(aux!=NULL)
    {
        cout<<aux->info<<" ";
        aux=aux->next;
    }
}

int caut_pozitie(int x)
{
    nod *aux=prim;
    int poz=1;
    while (aux!=NULL)
    {
        if (aux->info==x)
            return poz;
        poz++;
        aux=aux->next;
    }
    return -1;
}

int caut_valoare(int poz)
{
    int pozitie=1;
    nod *aux=prim;

    if(prim==NULL)
        return -1;

    while(aux!=NULL)
    {
      if(poz==pozitie)
                return aux->info;
      pozitie++;
      aux=aux->next;
    }
    return -1;
}

void sterg_valoare(int x)
{
    if(prim==NULL)
        return;
    if(x==prim->info)
    {
        if(x==ultim->info)
        {
            nod *aux=prim;
            delete aux;
            prim=NULL;
            ultim=NULL;
            return;
        }
        else
        {
            nod *aux=prim;
            prim=prim->next;
            delete aux;
            return;
        }
    }
    else
        if(x==ultim->info)
        {
            nod *aux=ultim;
            nod *p=prim;
            while(p->next!=ultim)
                p=p->next;
            delete aux;
            ultim=p;
            return;
        }
        nod *p=prim;
        while(p->next->info!=x)
            p=p->next;
        nod *aux=p->next;
        p->next=p->next->next;
        delete aux;
        return;
}

void sterg_pozitie(int poz)
{
    int n;

    if(prim==NULL)
        return;

    if(poz==1)
    {
        sterg_valoare(prim->info);
        return;
    }

    n=numar_noduri();

    if(poz==n)
    {
        sterg_valoare(ultim->info);
        return;
    }

    nod *p=prim;
    int pozitie=1;

    while(p!=NULL)
    {
        if(pozitie==poz-1)
        {
            nod *aux=p->next;
            delete aux;
            p->next=p->next->next;
            return;
        }
        pozitie++;
        p=p->next;

    }
}


int main()
{
   int option;

   while(1)
   {
       cout<<"Introduceti 1 pentru adaugare la final"<<endl;
       cout<<"Introduceti 2 pentru adaugare la inceput"<<endl;
       cout<<"Introduceti 3 pentru adaugare in interiorul listei pe o pozitie"<<endl;
       cout<<"Introduceti 4 pentru afisarea listei"<<endl;
       cout<<"Introduceti 5 pentru cautarea dupa valoare"<<endl;
       cout<<"Introduceti 6 pentru caurarea dupa pozitie"<<endl;
       cout<<"Introduceti 7 pentru stergerea dupa valoare"<<endl;
       cout<<"Introduceti 8 pentru stergerea dupa pozitie"<<endl;
       cout<<"Introduceti 9 pentru stergerea intregii liste"<<endl;

       cin>>option;

       switch(option)
       {
           case 1:{
                    int x;
                    cout<<"Introduceti x:";
                    cin>>x;
                    adaug_final(x);
                    break;
                  }
          case 2:{
                    int x;
                    cout<<"Introduceti x:";
                    cin>>x;
                    adaug_inceput(x);
                    break;
                }
        case 3:{int x,poz;
                cout<<"Introduceti x si pozitia lui";
                cin>>x>>poz;
                adaug_interior(x,poz);
                break;
                }
        case 4:{
                afisare();
                cout<<endl;
                break;
                }
        case 5:{
                int x;
                cout<<"Introduceti valoarea cautata:"<<endl;
                cin>>x;
                if (caut_pozitie(x)==-1)
                    cout<<"Elementul nu a fost gasit";
                else
                    cout<<"Elementul "<<x<<" se afla pe pozitia "<<caut_pozitie(x)<<endl;
                break;
                }
        case 6:{
                int poz;
                cout<<"Introduceti pozitia cautata:"<<endl;
                cin>>poz;
                if(caut_valoare(poz)==-1)
                    cout<<"Pozitia nu este valida";
                else
                    cout<<"Pe pozitia "<<poz<<"se afla"<< caut_valoare(poz)<<endl;
                break;
                }
        case 7:{
                int x;
                cout<<"Introduceti valoarea elementul dorit a fi sters"<<endl;
                cin>>x;
                sterg_valoare(x);
                break;
                }
        case 8:{
                int poz;
                cout<<"Introduceti pozitia elementului ce dorinti a fi sters"<<endl;
                cin>>poz;
                sterg_pozitie(poz);
                break;
                }
        case 9:{
                break;
                }
        default:{return 0;}

        }
    }

    return 0;
}
