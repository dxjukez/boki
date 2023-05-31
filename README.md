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
int main(void) {

  int dubina = 5;
  CvorBinarnogStabla *Stablo = NapraviBinarnoStablo(dubina);
  Prikazi_Stablo(Stablo, dubina);
  return 0;
}
