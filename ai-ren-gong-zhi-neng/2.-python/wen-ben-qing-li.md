# 文本清理

```
import re
import json
import os
import unicodedata
import string 
import re  # regular expression

json_o_filename = './output_files/json_file_original'
text_o_filename = './output_files/text_original.txt'
cleaned_text_filename = './output_files/cleaned_tweet_text.txt'

```

```
# read tweets from json file
def read_json_file(json_filename, json_file_number):
    json_file = json_filename + '_' + str(json_file_number) +'.txt'
    if os.path.exists(json_file):
        with open(json_file, 'r', encoding="utf-8") as f:
            json_string = f.read()
            parsed = json.loads(json_string)
    return parsed       
```

```
//# clearn text functions
def remove_at(text_sentence):
    text_out = re.sub("@\S+",'',text_sentence)
    return text_out

def remove_hashtag(text_sentence):
    text_out = re.sub("#\S+",'',text_sentence)
    return text_out

def remove_url(text_sentence):
    text_out = re.sub("https*\S+",'',text_sentence)
    return text_out

def remove_punctuation(text_sentence):
    text_out = re.sub('[%s]' % re.escape(string.punctuation),'',text_sentence)
    return text_out

def remove_number(text_sentence):
    text_out = re.sub(r'\w*\d+\w*','',text_sentence)
    return text_out

def remove_space(text_sentence):
    text_out = re.sub('\s{2,}','',text_sentence)
    text_out = text_out.strip()
    return text_out

def remove_others(text_sentence):
    text_out = text_sentence.replace('\r', '')     ## 回车符  win: \r\n
    text_out = text_sentence.replace('\r\n', '')     ## 回车符  win: \r\n
    text_out = text_sentence.replace('\t', ' ')    ## 水平制表符
    text_out = text_sentence.replace('\f', ' ')    ## 换页符
    return text_out

def remove_unicode(text_sentence):
    text_out = text_sentence.encode('ascii', 'ignore').decode() 
    return text_out

def join_multi_line(text_sentence):
    text_out = ''
    for line in text_sentence:
        text_out += line.strip('\n')
    return text_out
```

```
def clear_text(text):
    print('--------------- begin to clean ---------------\n')
    
    print('\n1- remove_at\n')   
    text = remove_at(text)
    print(text)
        
    print('\n2- remove_hashtag\n')
    text = remove_hashtag(text)
    print(text)
    
    print('\n3- remove_url\n')
    text = remove_url(text)
    print(text)
    
    print('\n4- remove_punctuation\n')
    text = remove_punctuation(text)
    print(text)
    
    print('\n5- remove_number\n')
    text = remove_number(text)
    print(text)
    
    print('\n6- remove_space\n')
    text = remove_space(text)
    print(text)
    
    print('\n7- remove_unicode\n')
    text = remove_unicode(text)
    print(text)
    
    print('\n8- remove_others\n')
    text = remove_others(text)
    print(text)
    
    print('\n9- join_multi_line\n')
    text = join_multi_line(text)
    print(text)
    
    print('\n--------------- end clean ----------------\n')
 
```

```
def save_o_text(filename, json_parsed, option):
    with open(filename, option, encoding="utf-8") as f:
        title = 'number | created time | text \n'
        f.write(title)
        number = 1
        for data in json_parsed:
            create_time = data['created_at']
            tweet_text = remove_space(data['text'])
            text = str(number) + ' | ' +  create_time[:10] + ' | ' + tweet_text +'\n'
            f.write(text)
            number = number + 1
            print(text)
            cleaned_text = clear_text(text)
            f.write('\n'+cleaned_text)
            
```

```
# start processing
json_file_number = 1
json_parsed = read_json_file(json_o_filename, json_file_number)
# print(json.dumps(json_parsed, indent=4, sort_keys=True))

save_o_text(text_o_filename, json_parsed['data'], 'w')
```

>
