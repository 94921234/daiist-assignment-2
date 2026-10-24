# Assignment 2 Report

*Delete this italic guidance as you fill in each section. You'll be asked to
defend any of this without your code in front of you. Write only what you
can actually explain.*

- **Name**: Ignacio Ascanio 
- **Student ID**: 18719
- **Email**: iascanio.ieu2022@student.ie.edu
- **Group**: BBADBA 5B

## Business problem, and the cost of being wrong

I chose to develop an AI-assisted road inspection system for a municipality responsible for maintaining road infrastructure.

Currently, road inspectors must examine photographs to identify damage such as potholes and cracks. This process can be time-consuming when dealing with large numbers of images.

My objective is to fine-tune a YOLO11 object detection model to identify different types of asphalt damage and help the municipality decide which images should be flagged for attention, reviewed manually, or passed.

The two main risks are missing actual road damage, which could delay maintenance and result in higher future costs, and incorrectly identifying damage, which could lead to unnecessary inspections.

The goal is not simply to maximize detection accuracy, but to find a balance between the financial cost of errors and the time required for human review.

## Dataset

I selected the **Asphalt Distress Detection** dataset from Roboflow Universe (workspace: `roboflow100vl-full`, project: `asphaltdistressdetection-shlk`, version 2).

The dataset contains 3,144 images divided into 2,200 training, 628 validation, and 316 test images. It contains six object detection classes: Alligator crack, Longitudinal crack, Oblique crack, Pothole, Repair, and Transverse crack.

Each image represents a photographed road surface, while the annotations identify the locations and types of visible asphalt conditions using bounding boxes.

This dataset fits my business problem because it allows a YOLO11 model to identify road surface damage that may require further inspection or maintenance. The Repair class represents previously repaired surfaces rather than necessarily indicating active damage.

A limitation identified during inspection is that some visually similar road scenes appear across the original dataset splits. I decided to retain these splits and document the potential impact on model evaluation, as reorganizing all related images would require substantial additional work.

### Dataset Exploration

Before training the model, I analyzed the class distribution, bounding-box sizes, and sample annotations to better understand the dataset.

The training set contains 3,478 annotated objects across six classes. However, the class distribution is highly imbalanced. Repair is the most represented class (1,157 annotations), followed by Transverse crack (919) and Longitudinal crack (911). In contrast, Pothole has only 63 annotations, making it the least represented class.

The bounding-box analysis showed a strongly right-skewed distribution, meaning that most annotated objects occupy relatively small portions of their images. This could make detection more challenging, especially for small potholes and thin cracks.

I also visualized a training image containing two potholes and confirmed that their bounding boxes appeared correctly positioned around the defects.

These observations suggest that overall detection metrics alone may not fully reflect the model's performance. Therefore, I will pay particular attention to per-class precision and recall, especially for underrepresented categories such as Pothole.


## What the dataset's composition implies

*Instances per class, train/val/test sizes, typical object size relative to the
image. Then the interpretation: which per-class numbers will be too thin to
trust, whether the test split resembles the images you'd really see, and what
you therefore can't conclude from your results.*

## Finetuning and baselines

*Which model and task (detection or classification) and why. A results table on
the same test split: your finetuned model, the pretrained COCO model zero-shot,
and a trivial baseline. What changed, and whether the gap is as large as you
expected.*

## Per-class evaluation and failure modes

*Per-class AP50, or per-class precision/recall and the confusion matrix.
Which class carries the headline number, which one is dragging, and what the
model actually confuses with what. Show where it breaks, not just that it
works.*

## Cost matrix and operating point

*Your euro cost for each kind of error and where each figure came from. The
base rate you assumed. The cost-against-threshold sweep, and the operating
point you chose. State the assumption that would most change your answer if it
were wrong.*

## What a person still has to check

*Your two thresholds, and what share of cases they send to a person. How many
that is per day, how long one takes to check, and how many people that needs.
Is that realistic? Then your design priced against the three alternatives:
doing nothing, having a person check everything, and letting the model decide
on its own.*

## Limitations & next steps

During dataset exploration, I identified a potential data leakage issue in the original Roboflow dataset. Although there were no exact duplicate images across the training, validation, and test splits, some images appeared to show nearly identical road scenes. This means the model could encounter very similar visual information during training and evaluation, potentially making its test performance more optimistic than its performance on completely new roads.

I considered reorganizing the dataset to address this issue, but identifying and grouping all related images would require substantial manual work, since the original image identities and scene groupings were not clearly available. Therefore, I decided to preserve the original dataset splits and acknowledge this limitation when interpreting the results.

Ideally, images belonging to the same original photograph or road scene should have been grouped together before splitting the dataset. Any data augmentation should then have been applied only to the training set. This would provide a more reliable evaluation of the model's ability to generalize to previously unseen road conditions.

## Generative AI use disclosure

*Per the syllabus AI Policy: what you used and how, or "no AI content used."*
