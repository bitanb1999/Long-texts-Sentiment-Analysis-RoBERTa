# Long-texts-Sentiment-Analysis-RoBERTa
PyTorch implementation of Sentiment Analysis of the long texts (Movie Reviews) written in Serbian language (which is underused language) using pretrained Multilingual RoBERTa based model (XLM-R) on the small dataset.
# Dataset
Dataset is balanced and it consits of only **1684** **positive** and **negative** reviews, and it can be found [here](https://github.com/vukbatanovic/SerbMR). This balanced dataset is a subset of unbalanced set of 4725 movie reviews. Reviews have scoring metric which ranges from 1 to 10. Scores from 1-4 are treated as negative, 5-6 as neutral and 7-10 as positive. So some reviews have very weak sentiment and are very hard to classify correctly even by humans. <br />
<details><summary>Click to see an example</summary>
<p>
  
`Braća Koen (Coen brothers) iako poznati po trilerima, oprobali su se više puta i u komedija, i postigli potpuni uspeh.
Ovaj film, pošto je kada se pojavio bio vrlo loše prihvaćen, nije nažalost uspeo da zablista po američkim bioskopima, ali je zato bio prava senzacija kad se pojavio na DVD-u i na osnovu toga, postao jedan od glavnih naslova u kolekciji svakog pravog filmofila.
„The Big Lebowski“ definitvno moj omiljeni film i jedino ostvarenje koje zaista uvek iznova i iznova mogu da gledam.
On predstavlja odu životnom stilu jednog pacifiste.
„The Big Lewbowski“ je klasična priča prevare, kriminala i spletkarenja viđena kroz oči skromnog čoveka, tačnije jednostavne individue sa vrlo malo prohteva, želja i ambicija.
Žanr ovog filma nije lako odrediti – može se reći da je komedija zbog svog izuzetno originalnog humorističkog sadržaja.
Bogat fantastičnim likovima i još boljim dijalozima, koji iako su se transformisali u besmrtne citate koji se koriste u svakodnevnom životu, „The Big Lebowski“ i pored ovih sada već navedenih segmenata ima još toliko mnogo toga da ponudi.
Gluma u ovom filmu je zaista neponovljiva.
Definitvni vrh karijere za Džefa Bridžesa (Jeff Bridges) i Džona Gudmana (John Goodman).
Iako su obojica vrhunska klasa glumaca, sa izvanrednim karijerama, nikada nisu uspeli da se udalje od ovih kultnih likova, a otuda i Bridžesu nadimak koji ga prati već deceniju i pratiće ga ceo život – The Dude.
Cela glumačka ekipa je izuzetnog kvaliteta.
Tu stoje još imena kao što su Stiv Bušemi (Steve Buscemi), Džon Torturo (John Turturro), Džulijana Mur (Julianne Moore) i Filip Sejmur Hofman (Philip Seymour Hoffman).
Svako je zaista uradio i više što se od njih moglo očekivati, a pogotovo Torturo koje je ovde stvario jednog od najzabavnijih epizodnih likova u svim filmovima ikada.
Po mom mišljenju, „The Big Lebowski“ je jedno od najvećih dostignuća u modernoj kinematografiji.
Ovaj projekat će vas nasmejati, zbuniti i zadiviti, i on se sa razlogom smatra za jednu od najcenjenijih komedija od strane većine kritičara.`
</p>
</details>
<br />

Dataset is splitted into train, val and test sets with percentages respectively: 80%, 10% and 10%, in the way that new sets are balanced.<br />

<p align="center">
<img src="garbage/1.png" width="900" height="250"/>
</p>

# Processing the data
Since we are dealing with large texts which probably can not fit into the [XLM-R](https://github.com/facebookresearch/XLM) model (which is around 512), we will need to split the text into multiple chunks and input each chunk separately through the model. After we get the vector representations of the contexts of each chunk we will join them into one with LSTM layer (there are various ways to do this, for example the easiest way would be to perform sum or average pooling on the contexts). I have chosen the LSTM out of learning purposes.<br /> 
We will examine the distribution of number of tokens of texts to see how we can split the text to fit into our model (we will use [XLM-R SentencePiece Tokenizer](https://huggingface.co/transformers/model_doc/xlmroberta.html#xlmrobertatokenizer) to tokenize the text):<br />

<p align="center">
<img src="garbage/2.png" width="700" height="500"/>
</p>
We can see that 1355 examples out of 1682 would not fit into our model! <br />
We will split the text into chunks of 200 words, where the starting 50 words of each chunk are the last 50 words of the previous chunk (overlap is 50).

# Model
In the notebook you can find the vectorized implementation of the complete model.
<p align="center">
<img src="garbage/model_sa_MR_complete.png" width="900" height="500"/>
</p>

# Results
The model achieves **79%** accuracy on the test set. Confusion matrix for the test set:

<p align="center">
<img src="garbage/4.png" />
</p>

State of the art result (86.11% cross-validation accuracy) for this task on the same dataset can be found [here](https://scindeks-clanci.ceon.rs/data/pdf/1821-3251/2017/1821-32511702104B.pdf). I believe that this score can be achieved with better tweaking of hyperparameters (in which I have not invested much time) and XLM-R large model (I have used base model, since the large model could not fit into my GPU). Also you have to keep in mind that the test score depends on sentiment intensity of examples in the test set. This is shown below.

<p align="center">
<img src="garbage/5.png" />
</p>

We can see that most of the wrongly classified examples have weak sentiment (score close to 5 and 6).

# Training curves
  
  Accuracy   |   Learning rate | Both
:-----------:|:---------------:|:-----:
<img src="garbage/6.png" width="700" height="250"/>  | <img src="garbage/7.png" width="700" height="250"/>  | <img src="garbage/8.png" width="700" height="250"/>

# Usage 
`git clone https://github.com/Data-Science-kosta/Long-texts-Sentiment-Analysis-RoBERTa`<br />
`cd Long-texts-Sentiment-Analysis-RoBERTa`<br />
 
 After that you can either upload entire folder on the Google Drive and run the notebook on Google Colaboratory or you can run it from the local machine and skip the *Connect to Google Drive section*.



