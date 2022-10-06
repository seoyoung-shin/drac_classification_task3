[MICCAI2022 DRAC AI Challenge task3] This is repository to share training and inference codes of Team KT_Bio_Health

동료들과 함께 역량향상의 기회로서 대회에 참여하게되었으며,</br>
안저영상의 병증 분류를 수행하는 TASK3 항목에서 팀 기준 최종 2위를 차지할 수 있었습니다.
|Team|Kappa|AUC|
|------|---|---|
|-|0.8910|0.9147|
|KT_Bio_Health|0.8902|0.9257|
|-|0.8761|0.8938|



## Directory Structure
다양한 시도를 했지만, 최종 구현된 코드만을 가지고 디렉토리를 구현했습니다. 
```bash
# code directory contains executable files for preprocessing and training.
code
├── util
│   ├── custom_augmentation.py
│   ├── make_k_fold_dataset.py
│   ├── upper_sampling_aug_random.py 
├── control_data_coloring.py
├── control_model.py 
├── tools.py
├── tools_criterion.py
├── tools_scheduler.py
├── train.py
├── submission.py
├── find_segmentation_file.py
└── voting_with_segmentation_new_rule.py

db

#db directory contains image files, csv about training data and submission.    
db
├── input
│   ├── train_images
│   ├── valid_images
├── submission_result.csv
└── train.csv
```
## Training & Submission
공개된 데이터는 총 611장의 데이터로 학습에 매우 적은 데이터라고 판단했습니다.</br>
때문에 데이터의 올바른 검증과 학습을 위해 K-Fold 기법을 사용하였으며, 랜덤하게 upper sampling을 수행하여 데이터를 증량했습니다.</br>
증량에 사용한 기법은 flip, rotation, zoom_in, bilateral 등의 기법을 랜덤하게 적용하는 방식입니다.</br>



```
# Pre-processing
python code/utils/make_k_fold_dataset.py   # 5-Fold
python code/upper_sampling_aug_random.py   # upsampling


# Training & Post-processing
python train.py # model=BeitLarge512, criterion=CELoss, optimizer=AdamW, scheduler=StepLR
python find_segmentation_file.py
python voting_with_segmentation_new_rule.py
```

병증 분류를 위해 입력 이미지에 대해 coloring 등의 기법을 추가하였고, BERT를 기반으로 한 BEiT모델을 사용했습니다. </br>
과정 중에 classification 학습만으로는 성능향상이 어렵다고 판단해 segmentation 후 그 결과를 추가로 활용하였습니다.</br>
이후 이미지 간의 비교를 통해 조금 더 세부적인 rule를 적용하여 최종 0.8902 kappa value를 얻을 수 있었습니다. </br>
</br>
인공지능 경진대회에 처음 도전해보며 많은 것을 배울 수 있었던 것 같습니다</br>
다음 기회에는 1등을 기약하며 열심히 분발해보려합니다..ㅋㅋ</br>
