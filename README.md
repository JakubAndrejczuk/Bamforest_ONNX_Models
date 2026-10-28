# Bamforest ONNX Models

**English** | [Polski](README.pl.md)

<img width="3543" height="1771" alt="background" src="https://github.com/user-attachments/assets/b85b46c4-e25b-45d5-adf4-0f292afaa61e" />

Two **Ultralytics YOLO26 nano** models for tree-crown detection and semantic segmentation in high-resolution RGB UAV imagery, trained using the BAMFORESTS dataset.

| Task | Model | Format |
| --- | --- | --- |
| Object detection | [`bamforest_y26n_detect`](models/bamforest_y26n_detect.onnx) | `.onnx` |
| Semantic segmentation | [`bamforest_y26n_sem`](models/bamforest_y26n_sem.onnx) | `.onnx` |

Model files are available in [`models/`](models/).

## Training data

[BAMFORESTS](https://www.dlr.de/en/eoc/about-us/remote-sensing-technology-institute/photogrammetry-and-image-analysis/public-datasets/bamforests) is a reference dataset of very-high-resolution RGB UAV imagery with annotated tree crowns. It covers forests and urban parks near Bamberg, Germany, with **27,160 annotated trees across 105 ha** ([Troles et al., 2024](https://doi.org/10.3390/rs16111935)). The models were trained on the **coco2048** release (`Bamberg_coco2048.zip`), containing **2048 × 2048 px** tiles and **COCO** annotations converted to Ultralytics format.

## Model performance

The models were trained with **Ultralytics**. The *nano* variant was chosen for its small size and low computational requirements.

| Model | Precision | Recall | mAP50 | mAP50–95 |
| --- | ---: | ---: | ---: | ---: |
| Detection | 76.1% | 68.0% | 74.3% | 45.3% |

**Semantic segmentation:** mIoU **76.8%**, pixel accuracy **83.6%**.

*Metrics were provided with the model training information and have not been independently revalidated.*

## QGIS Deepness

The models include default ONNX metadata for the **[QGIS Deepness](https://qgis-plugin-deepness.readthedocs.io/en/latest/)** plugin, following the [Deepness metadata guide](https://qgis-plugin-deepness.readthedocs.io/en/latest/creators/creators_add_metadata_to_model.html). In QGIS, open an RGB raster, launch Deepness and select the corresponding ONNX file from `models/`. Click **Reload Model** to load the model, then choose **Load default parameters** to apply the settings stored in its metadata. Review the settings before running inference.

| Setting | Detection | Semantic segmentation |
| --- | --- | --- |
| Model type | Detector | Segmentor |
| Resolution | 1.7 cm/px | 1.7 cm/px |
| Tile size | 2048 px | 2048 px |
| Batch size | 1 | 1 |
| Tiles overlap | 15% | 15% |
| Detector type | YOLO_Ultralytics | — |
| Confidence | 0.2 | — |
| IoU threshold | 0.5 | — |
| Class probability threshold | — | 0.5 |
| Remove small segment areas | — | 9 |

These settings are starting points; adjust them to your imagery for best results.

## Recommended conditions

The main purpose of these models is **initial tree-crown annotation when preparing custom training datasets**. Detection boxes and segmentation masks should be **manually checked and corrected** before being reused for training.

Recommended UAV acquisition conditions:

- GSD **1.5–3 cm/pixel**;
- calm weather;
- even illumination, preferably overcast skies;
- forward/side image overlap **at least 80%/80%**.

## License

Both ONNX files contain **AGPL-3.0** license metadata referring to the [Ultralytics license terms](https://www.ultralytics.com/license). Check the applicable terms before redistribution or commercial use. The BAMFORESTS dataset is governed by its own distribution and attribution conditions.
