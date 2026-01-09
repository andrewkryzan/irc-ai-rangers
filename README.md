# AI Rangers: Automating Trail Intelligence for Conservation

## Executive Summary

Irvine Ranch Conservancy (IRC) manages over 30,000 acres of protected land in Southern California and relies on trail cameras to monitor how that land is used. Those cameras generate hundreds of thousands of images each year, which were historically reviewed and classified by hand. That process was slow, inconsistent, and increasingly unsustainable as image volume grew.

This project replaces manual review with an automated computer vision pipeline that detects and classifies trail users into four categories: Pedestrian, Cyclist, Equestrian, and Vehicle. Using a custom-trained YOLOv5 model and a deployment workflow integrated with Addax AI, the system reduces manual image review by over 99 percent while providing structured, near–real-time reporting to support land management decisions.

---

## Problem Context

IRC processes more than 500,000 trail camera images annually across a geographically large and environmentally diverse area. Manual classification created several challenges:

- Review time scaled linearly with image volume  
- Classification quality varied across reviewers  
- Existing detection tools identified motion but not user type  
- Growing image volume made manual workflows impractical  

The goal was not just to detect activity, but to reliably differentiate who was using the trails and make that information usable for operations and planning.

---

## Business Questions

This project focused on answering three core questions:

- Can trail users be accurately classified at scale using computer vision?  
- Can an automated system replace manual review without sacrificing reliability?  
- How can model outputs be operationalized into reports that IRC staff can actually use?

---

## Data

- **Source:** IRC trail camera images  
- **Scale:** ~500,000 images collected annually  
- **Labeled subset:** ~1,300 images used for model training and evaluation  
- **Classes:** Pedestrian, Cyclist, Equestrian, Vehicle  

Images were labeled using Label Studio with bounding boxes. Due to data size and sensitivity, full image datasets are not included in this repository. Folder structure and workflows are documented instead.

---

## Approach

### Labeling

Images were labeled in Label Studio using a fixed class order to ensure consistency throughout training and deployment. Bounding-box annotations were exported in YOLO format.

### Model Development

A YOLOv5 architecture was selected and fine-tuned using transfer learning from COCO pre-trained weights. YOLOv5 was chosen intentionally due to its stability, lightweight architecture, and direct compatibility with Addax AI, which IRC uses for operational inference. While newer YOLO variants exist, YOLOv5 provided the best balance of performance, tooling maturity, and deployment reliability for this use case.

Training was conducted in a GPU-enabled environment using PyTorch. Data was split into train, validation, and test sets to avoid leakage and to separate model tuning from final evaluation.

### Deployment

The trained model was deployed through Addax AI, where it processes unlabeled images and produces structured outputs, including CSV reports and visual summaries that can be used directly by IRC staff.

---

## Results and Performance

The final model achieved strong performance across all four trail-user classes:

- Overall mAP@0.5 of approximately 96 percent  
- High precision and recall across dominant classes  
- Reliable differentiation between visually similar user types  

Performance varied slightly by class, reflecting natural class imbalance and environmental variation across camera locations. These tradeoffs were considered acceptable given the operational gains and were documented for future model improvements.

---

## Impact

Before automation, images were reviewed individually through a manual process that limited throughput and consistency. After deployment:

- Manual image review was reduced by over 99 percent  
- Large image batches can be processed in under an hour  
- Classification is consistent across time and locations  
- Trail usage data is available in structured, analyzable formats  

This enables IRC to shift staff time from manual review to higher-value analysis and decision making.

---

## Deliverables

- Trained YOLOv5 model weights  
- End-to-end labeling, training, and validation pipeline  
- Deployment workflow integrated with Addax AI  
- Performance metrics and visual diagnostics  
- Final project poster summarizing context, approach, and impact  

---

## Limitations and Future Work

- Class imbalance affects recall for less frequent trail users  
- Environmental conditions (lighting, foliage, occlusion) impact detection  
- Additional labeled data would further improve robustness  

Future work could include expanding the labeled dataset, incorporating temporal aggregation across image sequences, and evaluating newer YOLO variants if deployment constraints change.

---

## Repository Notes

Full image datasets are excluded due to size and data governance constraints. This repository focuses on code, documentation, model artifacts, and reproducible workflows.
