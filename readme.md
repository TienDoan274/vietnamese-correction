# Vietnamese-Corrector
### Spelling Correction based on [Pretrained BARTpho](https://github.com/VinAIResearch/BARTpho)

# Overview
## BARTpho
>We present BARTpho with two versions, BARTpho-syllable and BARTpho-word, which are the first public large-scale monolingual sequence-to-sequence models >pre-trained for Vietnamese. BARTpho uses the "large" architecture and the pre-training scheme of the sequence-to-sequence denoising autoencoder BART, >thus it is especially suitable for generative NLP tasks. We conduct experiments to compare our BARTpho with its competitor mBART on a downstream task of >Vietnamese text summarization and show that: in both automatic and human evaluations, BARTpho outperforms the strong baseline mBART and improves the >state-of-the-art. We further evaluate and compare BARTpho and mBART on the Vietnamese capitalization and punctuation restoration tasks and also find that >BARTpho is more effective than mBART on these two tasks.

For more details, look at the original [paper](https://arxiv.org/abs/2109.09701).

## My Model
My model is a pretrained model of ```vinai/bartpho-word``` in the dataset avaiable at [@duyvuleo/VNTC](https://github.com/duyvuleo/VNTC), including 10 topics: 
* Chinh tri Xa hoi
* Doi song
* Khoa hoc
* Kinh doanh
* Phap luat
* Suc khoe
* The gioi
* The thao
* Van hoa
* Vi tinh

# Usage
This model is avaiable in Huggingface at [bmd1905/vietnamese-corrector](https://huggingface.co/bmd1905/vietnamese-corrector), to quickly use my model, simply run:
```python
from transformers import pipeline

corrector = pipeline("text2text-generation", model="bmd1905/vietnamese-corrector")
```
```python
# Example
print(corrector("toi dang là sinh diên nem hay ở truong đạ hoc khoa jọc tự nhiên , trogn năm ke tiep toi sẽ chọn chuyen nganh về trí tue nhana tạo"))
print(corrector("côn viec kin doanh thì rất kho khan nên toi quyết dinh chuyển sang nghê khac  "))
print(corrector("một số chuyen gia tài chính ngâSn hànG của Việt Nam cũng chung quan điểmnày"))
print(corrector("Lần này anh Phươngqyết xếp hàng mua bằng được 1 chiếc"))
print(corrector("Nhưng sức huỷ divt của cơn bão mitch vẫn chưa thấm vào đâu lsovớithảm hoạ tại Bangladesh ăm 1970"))


```
```
Output:
- Tôi đang là sinh viên hay ở trường đại học khoa học tự nhiên, trong năm kế tiếp, tôi sẽ chọn chuyên ngành về trí tuệ nhân tạo.
- Công việc kinh doanh thì rất khó khăn nên tôi quyết định chuyển sang nghê khác.
- Một số chuyên gia tài chính ngân hàng của Việt Nam cũng chung quan điểm này.
- Lần này anh Phương quyết xếp hàng mua bằng được 1 chiếc.
- Nhưng sức huỷ diệt của cơn bão mitch vẫn chưa thấm vào đâu so với thảm hoạ tại Bangladesh năm 1970 .
```


# Training
First one, you need to install dependencies:
```
pip install -r requirements.txt
```
In case of pretraining on your own custom-dataset, you must modify the format of files the same with [data.vi.txt](https://github.com/bmd1905/Vietnamese-Corrector/blob/main/data/data.vi.txt). You then run the following script to create your dataset:
```
python generate_dataset.py --data path/to/data.txt --language 'vi' --model_name 'vinai/bartpho-word'
```
S.t.
* ```data```: path to your formated data.
* ```language```: a string to name your outputed file.
* ```model_name```: check wherever your sentences suitable with the model length, if not, remove it.

When you accomplished creating dataset, let train your model, simply run:
```
python /content/Vietnamese-Corrector/run_summarization.py \
      --model_name_or_path /models/my-custom-bartpho \
      --do_train \
      --do_eval \
      --evaluation_strategy="steps" \
      --eval_steps=10000 \
      --train_file /data/vi.train.csv \
      --validation_file /data/vi.test.csv \
      --output_dir ./models/my-custom-bartpho/ \
      --overwrite_output_dir \
      --per_device_train_batch_size=4 \
      --per_device_eval_batch_size=4 \
      --gradient_accumulation_steps=32 \
      --learning_rate="1e-7" \
      --num_train_epochs=2 \
      --predict_with_generate \
      --logging_steps="10" \
      --save_total_limit="2" \
      --max_target_length=1024 \
      --max_source_length=1024 \
      --fp16
```


# References
[1] [BARTpho](https://github.com/VinAIResearch/BARTpho) \
[2] [@oliverguhr/spelling](https://github.com/oliverguhr/spelling) \
[3] [@duyvuleo/VNTC](https://github.com/duyvuleo/VNTC)

