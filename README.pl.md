# Modele ONNX Bamforest

[English](README.md) | **Polski**

<img width="3543" height="1771" alt="background" src="https://github.com/user-attachments/assets/4b11b440-ea14-463c-b50d-42c3cf23699d" />

Dwa modele **Ultralytics YOLO26 nano** do detekcji i segmentacji semantycznej koron drzew na wysokorozdzielczych zobrazowaniach UAV RGB, wytrenowane na danych projektu BAMFORESTS.

| Zadanie | Model | Format |
| --- | --- | --- |
| Detekcja obiektów | [`bamforest_y26n_detect`](models/bamforest_y26n_detect.onnx) | `.onnx` |
| Segmentacja semantyczna | [`bamforest_y26n_sem`](models/bamforest_y26n_sem.onnx) | `.onnx` |

Pliki modeli znajdują się w folderze [`models/`](models/).

## Dane treningowe

[BAMFORESTS](https://www.dlr.de/en/eoc/about-us/remote-sensing-technology-institute/photogrammetry-and-image-analysis/public-datasets/bamforests) to referencyjny zbiór zobrazowań UAV RGB o bardzo wysokiej rozdzielczości z oznaczonymi koronami drzew. Obejmuje lasy i parki miejskie w okolicach Bambergu (Niemcy): **27 160 oznaczonych drzew na powierzchni 105 ha** ([Troles i in., 2024](https://doi.org/10.3390/rs16111935)). Modele wytrenowano z wykorzystaniem wersji **coco2048** (`Bamberg_coco2048.zip`), zawierającej kafle **2048 × 2048 px** i anotacje **COCO**, przekonwertowane do formatu Ultralytics.

## Parametry modeli

Modele wytrenowano na platformie **Ultralytics**. Wariant *nano* wybrano ze względu na niewielki rozmiar i niskie wymagania obliczeniowe.

| Model | Precision | Recall | mAP50 | mAP50–95 |
| --- | ---: | ---: | ---: | ---: |
| Detekcja | 76,1% | 68,0% | 74,3% | 45,3% |

**Segmentacja semantyczna:** mIoU **76,8%**, pixel accuracy **83,6%**.

*Metryki pochodzą z informacji o treningu przekazanych przez autorów modeli; nie zostały ponownie zweryfikowane.*

## QGIS Deepness

Modele przygotowano do użycia we wtyczce **[QGIS Deepness](https://qgis-plugin-deepness.readthedocs.io/en/latest/)**, zapisując parametry domyślne w metadanych ONNX zgodnie z [instrukcją Deepness](https://qgis-plugin-deepness.readthedocs.io/en/latest/creators/creators_add_metadata_to_model.html). W QGIS należy otworzyć raster RGB, uruchomić Deepness i wskazać odpowiedni plik z `models/`. Następnie kliknij **Reload Model**, aby wczytać model, i wybierz **Load default parameters**, aby zastosować parametry domyślne zapisane w jego metadanych. Przed uruchomieniem analizy sprawdź ustawienia.

| Parametr | Detekcja | Segmentacja semantyczna |
| --- | --- | --- |
| Model type | Detector | Segmentor |
| Resolution | 1,7 cm/px | 1,7 cm/px |
| Tile size | 2048 px | 2048 px |
| Batch size | 1 | 1 |
| Tiles overlap | 15% | 15% |
| Detector type | YOLO_Ultralytics | — |
| Confidence | 0,2 | — |
| IoU threshold | 0,5 | — |
| Class probability threshold | — | 0,5 |
| Remove small segment areas | — | 9 |

Parametry są wartościami początkowymi — zaleca się ich dostosowanie do przetwarzanych zobrazowań.

## Zalecane warunki korzystania

Głównym przeznaczeniem modeli jest **wstępne oznaczanie koron drzew podczas tworzenia własnych zbiorów treningowych**. Wyniki detekcji i segmentacji należy **zweryfikować i skorygować ręcznie** przed wykorzystaniem do treningu kolejnych modeli.

Zalecane warunki pozyskiwania zobrazowań UAV:

- GSD **1,5–3 cm/piksel**;
- bezwietrzna pogoda;
- równomierne oświetlenie, najlepiej pełne zachmurzenie;
- pokrycie podłużne/poprzeczne **co najmniej 80%/80%**.

## Licencja

Metadane obu modeli ONNX wskazują **AGPL-3.0** i odsyłają do [warunków licencji Ultralytics](https://www.ultralytics.com/license). Przed redystrybucją lub zastosowaniem komercyjnym należy sprawdzić właściwe warunki licencji. Dane BAMFORESTS podlegają odrębnym warunkom udostępniania określonym przez właścicieli zbioru.
