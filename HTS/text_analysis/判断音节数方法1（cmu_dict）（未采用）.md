背景：cmu_dict在每一个原音音素(phoneme)后面都有一个数字，辅音音素后没有 方法：通过数因素序列中的数字个数，即原音音素个数，判断音节(syllable)数

https://stackoverflow.com/questions/405161/detecting-syllables-in-a-word

from nltk.corpus import cmudict 
d = cmudict.dict() 
def nsyl(word): 
  return [len(list(y for y in x if y[-1].isdigit())) for x in d[word.lower()]]
