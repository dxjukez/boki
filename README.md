```c

#include <stdio.h>
#include <stdlib.h>

typedef struct CvorBinarnogStabla {
  int vrednost;
  struct CvorBinarnogStabla *levo;
  struct CvorBinarnogStabla *desno;
} CvorBinarnogStabla;

CvorBinarnogStabla *NapraviCvor(int vrednost) {

  CvorBinarnogStabla *NoviCvor =
      (CvorBinarnogStabla *)malloc(sizeof(CvorBinarnogStabla));
  NoviCvor->vrednost = vrednost;
  NoviCvor->levo = NULL;
  NoviCvor->desno = NULL;

  return NoviCvor;
}
CvorBinarnogStabla *NapraviBinarnoStablo(int dubina) {
  if (dubina <= 0) {
    return NULL;
  }

  CvorBinarnogStabla *glava = NapraviCvor(1);
  glava->levo = NapraviBinarnoStablo(dubina - 1);
  glava->desno = NapraviBinarnoStablo(dubina - 1);

  return glava;
}
void Prikazi_Stablo(CvorBinarnogStabla *koren, int dubina) {

  if (koren == NULL)
    return;

  printf("%d\n", koren->vrednost);
  Prikazi_Stablo(koren->levo, dubina + 1);
  Prikazi_Stablo(koren->desno, dubina + 1);
}
CvorBinarnogStabla *MaksimalnaVrednostUStablu(CvorBinarnogStabla *glava) {
  if (glava == NULL)
    return NULL;

  CvorBinarnogStabla *maksimalni = glava;

  CvorBinarnogStabla *Levo_Max = MaksimalnaVrednostUStablu(glava->levo);
  CvorBinarnogStabla *Desno_Max = MaksimalnaVrednostUStablu(glava->desno);

  if (Levo_Max != NULL && Levo_Max->vrednost > maksimalni->vrednost) {
    maksimalni = Levo_Max;
  }
  if (Desno_Max != NULL && Desno_Max->vrednost > maksimalni->vrednost) {
    maksimalni = Desno_Max;
  }

  return maksimalni;
}
void Izbrisi_Stablo(CvorBinarnogStabla *glava) {
  if (glava == NULL)
    return;

  Izbrisi_Stablo(glava->desno);
  Izbrisi_Stablo(glava->levo);
  printf("Brisem Cvor sa vrednoscu od %i\n", glava->vrednost);
  free(glava);
}
int Identicna_Stabla(CvorBinarnogStabla *glava1, CvorBinarnogStabla *glava2) {
  if (glava1 == NULL && glava2 == NULL)
    return 1;
  if (glava1 == NULL || glava2 == NULL)
    return 0;
  if (glava1->vrednost != glava2->vrednost)
    return 0;

  int levaIdenticka = Identicna_Stabla(glava1->levo, glava2->levo);
  int desnaIdenticka = Identicna_Stabla(glava1->desno, glava2->desno);

  return levaIdenticka && desnaIdenticka;
}


int main(void) {

  int dubina = 5;
  CvorBinarnogStabla *Stablo = NapraviBinarnoStablo(dubina);
  Prikazi_Stablo(Stablo, dubina);
  printf("\n----------\n");
  printf("%i", MaksimalnaVrednostUStablu(Stablo)->vrednost);
  printf("\n----------\nBrisanje Stabla...\n----------\n");
  Izbrisi_Stablo(Stablo);
  CvorBinarnogStabla *StabloDva = NapraviBinarnoStablo(dubina);
  CvorBinarnogStabla *StabloTri = NapraviBinarnoStablo(dubina);
  if (Identicna_Stabla(StabloDva, StabloTri) == 1) {
    printf("\n Stabla su identicna!\n");
  } else {
    printf("\n Stabla NISU identicna!\n");
  }
  
  Prikazi_Stablo(StabloTri, dubina);
  return 0;
}

```
# Dodaj Cvor U stablo
```c
CvorBinarnogStabla *NapraviCvorBSTIUpisiVrednost(int paPodatak) {

  CvorBinarnogStabla *NoviCvor =
      (CvorBinarnogStabla *)malloc(sizeof(CvorBinarnogStabla));

  NoviCvor->vrednost = paPodatak;

  NoviCvor->levo = NULL;
  NoviCvor->desno = NULL;

  return NoviCvor;
}
CvorBinarnogStabla *DodavanjeCvoraUBST(CvorBinarnogStabla *paKoren,
                                       int paPodatak) {

  if (!paKoren)
    return NapraviCvorBSTIUpisiVrednost(paPodatak);
  if (paPodatak < paKoren->vrednost)
    paKoren->levo = DodavanjeCvoraUBST(paKoren->levo, paPodatak);
  else
    paKoren->desno = DodavanjeCvoraUBST(paKoren->desno, paPodatak);
  return paKoren;
}
```
# Slicna Stabla
```c
int Slicna_Stabla(CvorBinarnogStabla* glava1, CvorBinarnogStabla* glava2) {
  if (glava1 == NULL && glava2 == NULL)
    return 1;

  if (glava1 == NULL || glava2 == NULL)
    return 0;

  int levaSlicna = Slicna_Stabla(glava1->levo, glava2->levo);
  int desnaSlicna = Slicna_Stabla(glava1->desno, glava2->desno);

  return levaSlicna && desnaSlicna;
}

```
# Korigovanje Nebalansiranog BST

```c

TipCvorBinarnogStabla *korigujStrukturuNebalansiranogBST(TipCvorBinarnogStabla *nizAdresaCvorovaBST[],
                                                        int prviElement, int poslednjiElement)
{
    if (prviElement > poslednjiElement)
        return NULL;

    int srednjiElement = (prviElement + poslednjiElement) / 2;

    TipCvorBinarnogStabla *korenBalansiranogBST = nizAdresaCvorovaBST[srednjiElement];

    korenBalansiranogBST->Levi = korigujStrukturuNebalansiranogBST(nizAdresaCvorovaBST,
                                                                  prviElement, srednjiElement - 1);

    korenBalansiranogBST->Desni = korigujStrukturuNebalansiranogBST(nizAdresaCvorovaBST,
                                                                   srednjiElement + 1, poslednjiElement);

    return korenBalansiranogBST;
}

```
# Kreiranje Balansiranog BST
```c

TipCvorBinarnogStabla *kreirajBalansiraniBST(TipCvorBinarnogStabla *koren, unsigned brojCvorovaUBST)
{
    TipCvorBinarnogStabla **nizAdresaCvorovaBST = (TipCvorBinarnogStabla **)
                                                   malloc(brojCvorovaUBST * sizeof(TipCvorBinarnogStabla *));

    unsigned indeksElementaNizaAdresaCvorovaBST = 0;

    popuniNizAdresaCvorovaBST(koren, nizAdresaCvorovaBST, &indeksElementaNizaAdresaCvorovaBST);

    TipCvorBinarnogStabla *korenBalansiranogBST =
                            korigujStrukturuNebalansiranogBST(nizAdresaCvorovaBST, 0, brojCvorovaUBST - 1);

    free(nizAdresaCvorovaBST);

    return korenBalansiranogBST;
}

```
