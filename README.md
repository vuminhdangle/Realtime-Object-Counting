# Real-Time Object Counting with YOLO11x, ROI Filtering, and Line Crossing

This is a computer vision project for **detecting, tracking, and counting moving objects in video**. The proof of concept focuses on apples moving along a conveyor belt, but the same processing workflow can be adapted to other classes, for examples: cars, motorcycles, with appropriate data and model configuration. 

How does it work? Rather than counting every detection in every frame, the system follows individual object across frames and increments a counter when a tracked object crosses a selected virtual line inside a ROI. 

## My objectives

- Understand the complete workflow from video processing and annotation to model a fine-tuning and video inference. 
- Learn how object detection and multi-object tracking** work together, and why persistent tracking identities are essential for counting. 
- Study polygonal ROI filtering to constrain computation to the relevant part of a scene. 
- Implement a virtual line-crossing detection using object trajectories and elementary geometry. 
- Using smoothing bounding-box centers to reduce false and repeated counts, recording already-counted track IDs. 
- Recognize limitations caused by startup coditions, overlapping objects, perspective, tracking instability, and non-independent validation data. 

## Processing Pipeline

```text
Source video
    v
Video conversion and frame extraction (FFmpeg, OpenCV)
    v
Single-frame annotation (Roboflow) and convert to YOLO labels
    v
Annotation inspection + image augmentation 
    v
Fine-tune pretrained YOLO11x
    v
Video frames -> polygonal ROI mask -> object detection and tracking
    v
Tracked bounding-box centers -> temporal center smoothing
    v
Virtual line-crossing test + already-counted ID check
    v
Annotated video display + cumulative object count
```

## Repository Structure

```text
Realtime-Object-Counting/
├── Code/
│   ├── 0_check_annotation.ipynb
│   ├── 1_data_preparation.ipynb
│   ├── 2.train_model.ipynb
│   └── 3.realtime-processing.ipynb
├── dataset/
│   ├── data.yaml
│   ├── train/              # Local training images and labels
│   │   ├── images/
│   │   └── labels/
│   ├── valid/              # For a future independent validation split
│   └── test/               # Evaluation data
├── Video/                  # Local source video, extracted frames, output video
├── requirements.txt
└── README.md
```

- The Python virtual environment, local videos and image datasets, temporary files, experiment runs, and large model weights are intentionally excluded from the public repository. 

- The original pretrained `yolo11x.pt` checkpoint is available from [Ultralytics YOLO11x](https://platform.ultralytics.com/ultralytics/yolo11/yolo11x).

[continue]
