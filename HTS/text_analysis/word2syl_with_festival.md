The way Festival segments syllables & problem

http://www.speech.zone/forums/topic/the-way-festival-segments-syllables/

In the lab session, when I looked up the pronunciation for a word using “lex.lookup”, I found that the response that Festival gave me was quite different from how we usually construct syllables in a word. For example, Festival would say (“caterpillar” nil (((k ae t) 1) ((ax p) 0) ((ih l) 1) ((er) 0)))(the example on the tutorial sheet), but according to maximal onset principle, [t] should be the onset of the second syllable. So why does Festival group phones like that? And does “1” refer to a stress?

Festival has its own algorithm for syllabifying phoneme sequences that have been predicted by the letter-to-sound module. This algorithm does not follow the “maximum onset” principal, and exactly what the method does is a bit obscure to me (I didn’t write that part of Festival).

The method for old diphones voices is different to that for newer unit selection voices (such as the one you will be using for the assessed practical).

The numbers 0, 1, 2 appended to phoneme names refer to lexical stress.

0 = unstressed
1 = primary stress
2 = secondary stress

For phoneme sequences predicted by letter-to-sound, sometimes only 0 and 1 are used, so you might find more than one syllable with stress of “1”.

Syllables are indicated by the bracketing: in the example above, the first syllable is (k ae t) and it has a stress of “1”
