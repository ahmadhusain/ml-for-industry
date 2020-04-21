# Retail



<style>
body {
text-align: justify}
</style>



##  E-Commerce Clothing Reviews

### Background

Perkembangan teknologi membuat pergeseran perilaku customer dari pembelian offline menjadi pembelian online atau melalui e-commerce. 
Perbedaan utama saat berbelanja secara online atau offline adalah
saat akan berbelanja secara online, calon customer tidak dapat memeriksa barang yang akan dibeli secara langsung dan biasanya dibantuk oleh gambar atau deskripsi yang diberikan oleh penjual.
Tentunya customer akan mencari informasi mengenai produk yang akan dibeli untuk meminimalisir dampak negatif yang didapat. Untuk membantu customer dalam menentukan product yang akan dibeli, mayoritas e-commerce sekarang ini menyediakan fitur online customer review, dimana online customer review ini dijadikan sebagai salah satu media customer mendapatkan informasi tentang produk dari customer yang telah membeli produk tersebut. Meningkatnya e-commerce di Indonesia, kebutuhan analisa mengenai online customer review dirasa perlu dilakukan untuk mendukung agar customer dapat memiliki pengalaman belanja online yang lebih baik daripada belanja offline. Salah satu implementasi data review customer tersebut dapat dimanfaatkan untuk membuat model yang dapat memprediksi apakah product tersebut direkomendasikan atau tidak direkomendasikan. Harapannya setelah perusahaan dapat menilai product mana yang direkomendasikan dan yang tidak direkomendasikan, dapat membantu perusahaan dalam pertimbangan penentuan top seller. Untuk seller yang memiliki banyak product yang direkomendasikan, dapat dijadikan sebagai top seller.



```r
reviews <- read.csv("assets/03- retail/Womens Clothing E-Commerce Reviews.csv")
head(reviews)
```

```
#>   X Clothing.ID Age                   Title
#> 1 0         767  33                        
#> 2 1        1080  34                        
#> 3 2        1077  60 Some major design flaws
#> 4 3        1049  50        My favorite buy!
#> 5 4         847  47        Flattering shirt
#> 6 5        1080  49 Not for the very petite
#>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            Review.Text
#> 1                                                                                                                                                                                                                                                                                                                                                                                                                                                                Absolutely wonderful - silky and sexy and comfortable
#> 2                                                                                                                                                                                                      Love this dress!  it's sooo pretty.  i happened to find it in a store, and i'm glad i did bc i never would have ordered it online bc it's petite.  i bought a petite and am 5'8".  i love the length on me- hits just a little below the knee.  would definitely be a true midi on someone who is truly petite.
#> 3 I had such high hopes for this dress and really wanted it to work for me. i initially ordered the petite small (my usual size) but i found this to be outrageously small. so small in fact that i could not zip it up! i reordered it in petite medium, which was just ok. overall, the top half was comfortable and fit nicely, but the bottom half had a very tight under layer and several somewhat cheap (net) over layers. imo, a major design flaw was the net over layer sewn directly into the zipper - it c
#> 4                                                                                                                                                                                                                                                                                                                                                                                         I love, love, love this jumpsuit. it's fun, flirty, and fabulous! every time i wear it, i get nothing but great compliments!
#> 5                                                                                                                                                                                                                                                                                                                     This shirt is very flattering to all due to the adjustable front tie. it is the perfect length to wear with leggings and it is sleeveless so it pairs well with any cardigan. love this shirt!!!
#> 6             I love tracy reese dresses, but this one is not for the very petite. i am just under 5 feet tall and usually wear a 0p in this brand. this dress was very pretty out of the package but its a lot of dress. the skirt is long and very full so it overwhelmed my small frame. not a stranger to alterations, shortening and narrowing the skirt would take away from the embellishment of the garment. i love the color and the idea of the style but it just did not work on me. i returned this dress.
#>   Rating Recommended.IND Positive.Feedback.Count  Division.Name Department.Name
#> 1      4               1                       0      Initmates        Intimate
#> 2      5               1                       4        General         Dresses
#> 3      3               0                       0        General         Dresses
#> 4      5               1                       0 General Petite         Bottoms
#> 5      5               1                       6        General            Tops
#> 6      2               0                       4        General         Dresses
#>   Class.Name
#> 1  Intimates
#> 2    Dresses
#> 3    Dresses
#> 4      Pants
#> 5    Blouses
#> 6    Dresses
```

Data yang digunakan merupakan data [women e-commerce clothing reviews](https://www.kaggle.com/nicapotato/womens-ecommerce-clothing-reviews). Terdapat dua variabel yang menjadi fokus analisis ini yaitu `Review.Text` dan `Recommended.IND`. Variabel `Review.Text` merupakan review yang diberikan oleh customer terhadap product dari berbagai e-commerce, sedangkan `Recommended.IND` merupakan penilaian rekomendasi dari customer, `1` artinya product tersebut `recommended` dan `0` artinya product tersebut `not recommended`.

Sebelum masuk cleaning data, kita ingin mengetahui proporsi dari target variabel:

```r
prop.table(table(reviews$Recommended.IND))
```

```
#> 
#>         0         1 
#> 0.1776377 0.8223623
```

### Cleaning Data

Untuk mengolah data text, kita perlu mengubah data teks dari vector menjadi corpus dengan function `Vcorpus()`.


```r
reviews_corpus <- VCorpus(VectorSource(reviews$Review.Text))
reviews_corpus
```

```
#> <<VCorpus>>
#> Metadata:  corpus specific: 0, document level (indexed): 0
#> Content:  documents: 23486
```

Selanjutnya, kita melakukan text cleansing dengan beberapa langkah sebagai berikut:

- `tolower` digunakan untuk mengubah semua karakter menjadi lowercase.
- `removePunctuation` digunakan untuk menghilangkan semua tanda baca.
- `removeNumbers` digunakan untuk menghilangkan semua angka.
- `stopwords` digunakan untuk menghilangkan kata-kata umum (am,and,or,if).
- `stripWhitespace` digunakan untuk menghapus karakter spasi yang berlebihan.


```r
data_clean <- reviews_corpus %>% 
  tm_map(content_transformer(tolower)) %>% 
  tm_map(removePunctuation) %>% 
  tm_map(removeNumbers) %>% 
  tm_map(removeWords, stopwords("en")) %>% 
  tm_map(content_transformer(stripWhitespace)) 
inspect(data_clean[[1]])
```

```
#> <<PlainTextDocument>>
#> Metadata:  7
#> Content:  chars: 43
#> 
#> absolutely wonderful silky sexy comfortable
```

Setelah melakukan text cleansing, text tersebut akan diubah menjadi Document Term Matrix(DTM) melalui proses tokenization. Tokenization berfungsi memecah 1 teks atau kalimat menjadi beberapa term. Terim bisa berupa 1 kata, 2 kata, dan seterusnya. Pada format DTM, 1 kata akan menjadi 1 feature, secara default nilainya adalah jumlah kata pada dokumen tersebut. 

```r
dtm_text <- DocumentTermMatrix(data_clean)
```

Sebelum membentuk model, tentunya kita perlu split data menjadi data train dan data test dengan proporsi 80:20.

```r
set.seed(100)
idx <- sample(nrow(dtm_text), nrow(dtm_text)*0.8)
train <- dtm_text[idx,]
test <- dtm_text[-idx,]
train_label <- reviews[idx,"Recommended.IND"]
test_label <-  reviews[-idx,"Recommended.IND"]
```

Term yang digunakan pada model ini, kita hanya mengambil term yang muncul paling sedikit 100 kali dari seluruh observasi dengan `findFreqTerms()`.

```r
freq <- findFreqTerms(dtm_text, 100)
train_r <- train[, freq]
test_r <- test[, freq]

inspect(train_r)
```

```
#> <<DocumentTermMatrix (documents: 18788, terms: 870)>>
#> Non-/sparse entries: 389603/15955957
#> Sparsity           : 98%
#> Maximal term length: 13
#> Weighting          : term frequency (tf)
#> Sample             :
#>        Terms
#> Docs    dress fabric fit great just like love size top wear
#>   12348     0      1   0     1    0    1    0    1   0    1
#>   12812     0      1   0     1    0    0    1    0   0    0
#>   15905     0      1   2     1    0    1    0    2   0    1
#>   1775      3      0   0     0    0    0    1    3   3    1
#>   18527     0      1   0     1    1    0    0    0   2    0
#>   19547     4      0   1     0    0    0    0    0   0    0
#>   21091     0      0   0     0    0    1    0    2   0    1
#>   22039     1      0   1     0    0    2    0    1   2    1
#>   4789      1      0   2     0    0    1    0    1   0    1
#>   6317      0      0   0     1    1    2    0    0   2    0
```

Nilai dari setiap matrix masih berupa angka numerik, dengan range 0-inf. Naive bayes akan memiliki performa lebih bagus ketika variabel numerik diubah menjadi kategorik. Salah satu caranya dengan Bernoulli Converter, yaitu jika jumlah kata yang muncul lebih dari 1, maka kita akan anggap nilainya adalah 1, jika 0 artinya tidak ada kata tersebut.

```r
bernoulli_conv <- function(x){
  x <- as.factor(ifelse(x > 0, 1, 0))
  return(x)
}

train.bern <- apply(train_r, MARGIN = 2, FUN = bernoulli_conv)
test.bern <- apply(test_r, MARGIN = 2, FUN = bernoulli_conv)
```

### Modelling

Selanjutnya, pembentukan model menggunakan naive bayes dan diikuti dengan prediksi data test.

```r
model.nb <- naiveBayes(x = train.bern, 
                       y = as.factor(train_label), 
                       laplace = 1)
pred.nb <- predict(object = model.nb, newdata= test.bern)
```

Dai hasil prediksi data test, kita akan menampilkan Confusion Matrix untuk mengetahui performa model.

```r
confusionMatrix(data = as.factor(pred.nb),
                reference = as.factor(test_label),
                positive = "1")
```

```
#> Confusion Matrix and Statistics
#> 
#>           Reference
#> Prediction    0    1
#>          0  614  365
#>          1  253 3466
#>                                                
#>                Accuracy : 0.8685               
#>                  95% CI : (0.8585, 0.878)      
#>     No Information Rate : 0.8155               
#>     P-Value [Acc > NIR] : < 0.00000000000000022
#>                                                
#>                   Kappa : 0.5837               
#>                                                
#>  Mcnemar's Test P-Value : 0.000008004          
#>                                                
#>             Sensitivity : 0.9047               
#>             Specificity : 0.7082               
#>          Pos Pred Value : 0.9320               
#>          Neg Pred Value : 0.6272               
#>              Prevalence : 0.8155               
#>          Detection Rate : 0.7378               
#>    Detection Prevalence : 0.7916               
#>       Balanced Accuracy : 0.8065               
#>                                                
#>        'Positive' Class : 1                    
#> 
```

### Visualize Data Text

Selanjutnya, kita akan coba lakukan prediksi terhadap data test dan juga menampilkan visualisasi text tersebut menggunakan package lime.


```r
set.seed(100)
idx <- sample(nrow(reviews), nrow(reviews)*0.8)
train_lime <- reviews[idx,]
test_lime <- reviews[-idx,]
```


```r
tokenize_text <- function(text){
  
  #create corpus
  
  data_corpus <- VCorpus(VectorSource(text))
  
  # cleansing
  data_clean <- data_corpus %>% 
  tm_map(content_transformer(tolower)) %>% 
  tm_map(removePunctuation) %>% 
  tm_map(removeNumbers) %>% 
  tm_map(removeWords, stopwords("en")) %>% 
  tm_map(content_transformer(stripWhitespace)) 
  
  #dtm
  dtm_text <- DocumentTermMatrix(data_clean)

  #convert to bernoulli
  data_text <- apply(dtm_text, MARGIN = 2, FUN = bernoulli_conv)
  
  return(data_text)
}
```



```r
model_type.naiveBayes <- function(x){
  return("classification")
}

predict_model.naiveBayes <- function(x, newdata, type = "raw") {

    # return classification probabilities only   
    res <- predict(x, newdata, type = "raw") %>% as.data.frame()
    
    return(res)
}

text_train <- train_lime$Review.Text %>% 
              as.character()
```


```r
explainer <- lime(text_train,
                  model = model.nb,
                  preprocess = tokenize_text)
```


```r
text_test <- test_lime$Review.Text %>% 
            as.character()

set.seed(100)
explanation <- explain(text_test[5:10],
                       explainer = explainer,
                       n_labels =1,
                       n_features = 50,
                       single_explanation = F)
```


```r
plot_text_explanations(explanation)
```

<!--html_preserve--><div id="htmlwidget-90646d04481062629235" style="width:100%;height:auto;" class="plot_text_explanations html-widget"></div>
<script type="application/json" data-for="htmlwidget-90646d04481062629235">{"x":{"html":"<div style=\"overflow-y:scroll;font-family:sans-serif;height:100%\"> <p> <span class='negative_1'>Just<\/span> <span class='negative_1'>ordered<\/span> <span class='positive_1'>this<\/span> <span class='negative_1'>in<\/span> a <span class='negative_1'>small<\/span> <span class='negative_1'>for<\/span> <span class='negative_1'>me<\/span> (<span class='negative_1'>5'6<\/span>\", <span class='positive_1'>135<\/span>, <span class='positive_1'>size<\/span> <span class='positive_1'>4<\/span>) <span class='negative_1'>and<\/span> <span class='positive_1'>medium<\/span> <span class='negative_1'>for<\/span> <span class='negative_1'>my<\/span> mom (<span class='positive_1'>5'3<\/span>\", <span class='positive_1'>130<\/span>, <span class='positive_1'>size<\/span> 8) <span class='negative_1'>and<\/span> <span class='positive_1'>it<\/span> <span class='positive_1'>is<\/span> <span class='positive_1'>gorgeous<\/span> - <span class='positive_1'>beautifully<\/span> draped, <span class='negative_1'>all<\/span> <span class='positive_1'>the<\/span> <span class='positive_1'>weight<\/span>/warmth <span class='negative_1'>i'll<\/span> <span class='positive_1'>need<\/span> <span class='negative_1'>for<\/span> houston <span class='positive_1'>fall<\/span> <span class='negative_1'>and<\/span> <span class='positive_1'>winter<\/span>, <span class='negative_1'>looks<\/span> <span class='negative_1'>polished<\/span> <span class='positive_1'>snapped<\/span> <span class='positive_1'>or<\/span> <span class='negative_1'>unsnapped<\/span>. <span class='positive_1'>age<\/span>-<span class='positive_1'>appropriate<\/span> <span class='negative_1'>for<\/span> <span class='negative_1'>both<\/span> <span class='negative_1'>my<\/span> mom (<span class='positive_1'>60<\/span>'<span class='negative_1'>s<\/span>) <span class='negative_1'>and<\/span> myself (<span class='negative_1'>30<\/span>'<span class='negative_1'>s<\/span>). <span class='positive_1'>will<\/span> <span class='negative_1'>look<\/span> <span class='positive_1'>amazing<\/span> with <span class='positive_1'>skinny<\/span> <span class='positive_1'>jeans<\/span> <span class='positive_1'>or<\/span> <span class='positive_1'>leggings<\/span>. <span class='positive_1'>we<\/span> <span class='negative_1'>ordered<\/span> <span class='positive_1'>the<\/span> <span class='positive_1'>gray<\/span> <span class='negative_1'>which<\/span> <span class='positive_1'>is<\/span> <span class='positive_1'>true<\/span> <span class='negative_1'>to<\/span> <span class='positive_1'>the<\/span> <span class='negative_1'>photos<\/span>. <\/br> <sub>Label predicted: 1 (100%)<br/>Explainer fit: 0.42<\/sub> <\/p><br/><p> <span class='positive_1'>Super<\/span> <span class='negative_1'>cute<\/span> <span class='positive_1'>and<\/span> <span class='positive_2'>comfy<\/span> <span class='negative_1'>pull<\/span> <span class='negative_1'>over<\/span>. <span class='negative_1'>sizing<\/span> <span class='negative_1'>is<\/span> <span class='negative_1'>accurate<\/span>. <span class='negative_1'>material<\/span> <span class='positive_1'>has<\/span> <span class='positive_1'>a<\/span> <span class='positive_1'>little<\/span> <span class='positive_1'>bit<\/span> <span class='positive_1'>of<\/span> <span class='positive_1'>stretch<\/span>. <\/br> <sub>Label predicted: 1 (96.31%)<br/>Explainer fit: 0.89<\/sub> <\/p><br/><p> <span class='positive_1'>Great<\/span> <span class='positive_1'>casual<\/span> <span class='negative_1'>top<\/span> <span class='positive_1'>with<\/span> <span class='positive_1'>flare<\/span>. <span class='negative_1'>looks<\/span> <span class='negative_1'>cute<\/span> <span class='positive_1'>with<\/span> <span class='positive_1'>grey<\/span> <span class='positive_1'>pilcro<\/span> <span class='negative_1'>stet<\/span> <span class='positive_1'>jeans<\/span>. <span class='positive_1'>flattering<\/span> <span class='positive_1'>with<\/span> <span class='negative_1'>peplum<\/span> <span class='negative_1'>in<\/span> <span class='negative_1'>back<\/span>. <span class='positive_1'>nice<\/span> <span class='negative_1'>cut<\/span> <span class='positive_1'>for<\/span> <span class='negative_1'>shoulders<\/span> <span class='positive_1'>and<\/span> <span class='negative_1'>neckline<\/span>. <\/br> <sub>Label predicted: 1 (99.08%)<br/>Explainer fit: 0.76<\/sub> <\/p><br/><p> <span class='positive_1'>This<\/span> <span class='negative_1'>poncho<\/span> <span class='positive_1'>is<\/span> <span class='negative_1'>so<\/span> <span class='positive_1'>cute<\/span> <span class='negative_1'>i<\/span> <span class='positive_1'>love<\/span> <span class='negative_1'>the<\/span> <span class='positive_1'>plaid<\/span> <span class='negative_1'>check<\/span> <span class='negative_1'>design<\/span>, <span class='negative_1'>the<\/span> <span class='positive_1'>colors<\/span> <span class='negative_1'>look<\/span> <span class='negative_1'>like<\/span> <span class='negative_1'>sorbet<\/span> & <span class='positive_1'>cream<\/span> <span class='negative_1'>and<\/span> <span class='positive_1'>it<\/span> <span class='positive_1'>will<\/span> <span class='positive_1'>pair<\/span> <span class='positive_1'>well<\/span> <span class='negative_1'>with<\/span> <span class='positive_1'>a<\/span> <span class='negative_1'>turtleneck<\/span> <span class='negative_1'>and<\/span> <span class='positive_1'>jeans<\/span> <span class='positive_1'>or<\/span> <span class='positive_1'>pencil<\/span> <span class='positive_1'>skirt<\/span> <span class='negative_1'>and<\/span> <span class='positive_1'>heels<\/span>. <span class='negative_1'>i<\/span> <span class='positive_1'>love<\/span> <span class='negative_1'>this<\/span> <span class='negative_1'>look<\/span> <span class='positive_1'>for<\/span> <span class='positive_1'>fall<\/span> <span class='negative_1'>and<\/span> <span class='positive_1'>it<\/span> <span class='positive_1'>can<\/span> <span class='negative_1'>roll<\/span> <span class='positive_1'>right<\/span> <span class='positive_1'>into<\/span> <span class='positive_1'>spring<\/span>. <span class='positive_1'>great<\/span> <span class='positive_1'>buy<\/span>!! <\/br> <sub>Label predicted: 1 (100%)<br/>Explainer fit: 0.37<\/sub> <\/p><br/><p> <span class='negative_1'>Tried<\/span> <span class='positive_1'>this<\/span> <span class='negative_1'>on<\/span> <span class='positive_1'>today<\/span> <span class='positive_1'>at<\/span> <span class='negative_1'>my<\/span> <span class='positive_1'>local<\/span> <span class='negative_1'>retailer<\/span> <span class='negative_1'>and<\/span> <span class='positive_1'>had<\/span> <span class='negative_1'>to<\/span> <span class='positive_1'>have<\/span> it. it <span class='negative_1'>is<\/span> <span class='positive_1'>so<\/span> <span class='positive_1'>comfortable<\/span> <span class='negative_1'>and<\/span> <span class='positive_1'>flattering<\/span>. <span class='positive_1'>it's<\/span> <span class='negative_1'>too<\/span> <span class='negative_1'>bad<\/span> <span class='negative_1'>the<\/span> <span class='negative_1'>picture<\/span> <span class='negative_1'>online<\/span> <span class='negative_1'>has<\/span> <span class='negative_1'>the<\/span> <span class='negative_1'>model<\/span> tucking it <span class='positive_1'>into<\/span> <span class='negative_1'>the<\/span> <span class='positive_1'>skirt<\/span> <span class='positive_1'>because<\/span> you <span class='positive_1'>can't<\/span> <span class='negative_1'>see<\/span> <span class='negative_1'>the<\/span> ruching <span class='negative_1'>across<\/span> <span class='negative_1'>the<\/span> <span class='negative_1'>front<\/span>. <span class='positive_1'>a<\/span> <span class='positive_1'>little<\/span> <span class='positive_1'>dressier<\/span> <span class='negative_1'>alternative<\/span> <span class='negative_1'>to<\/span> <span class='positive_1'>a<\/span> <span class='negative_1'>plain<\/span> <span class='positive_1'>tee<\/span> <span class='negative_1'>and<\/span> <span class='negative_1'>reasonably<\/span> <span class='negative_1'>priced<\/span> <span class='positive_1'>for<\/span> <span class='negative_1'>retailer<\/span>. 5'8\"\" <span class='negative_1'>and<\/span> <span class='positive_1'>i<\/span> <span class='negative_1'>generally<\/span> <span class='positive_1'>wear<\/span> <span class='positive_1'>a<\/span> <span class='positive_1'>6<\/span>, <span class='negative_1'>the<\/span> <span class='negative_1'>small<\/span> fit <span class='positive_1'>well<\/span>. <span class='positive_1'>will<\/span> <span class='positive_1'>probably<\/span> be <span class='negative_1'>back<\/span> <span class='positive_1'>for<\/span> <span class='negative_1'>the<\/span> <span class='positive_1'>black<\/span>! <\/br> <sub>Label predicted: 1 (57.75%)<br/>Explainer fit: 0.95<\/sub> <\/p><br/><p> <span class='negative_1'>I<\/span> <span class='negative_1'>bought<\/span> <span class='positive_1'>this<\/span> <span class='positive_1'>item<\/span> <span class='negative_1'>from<\/span> <span class='positive_1'>online<\/span>... <span class='positive_1'>the<\/span> <span class='positive_1'>fit<\/span> <span class='positive_1'>on<\/span> <span class='positive_1'>the<\/span> <span class='positive_1'>model<\/span> <span class='positive_1'>looked<\/span> <span class='positive_1'>a<\/span> <span class='negative_1'>little<\/span> <span class='negative_1'>loose<\/span> <span class='negative_1'>but<\/span> <span class='negative_1'>when<\/span> <span class='negative_1'>i<\/span> <span class='negative_1'>got<\/span> <span class='negative_1'>mine<\/span> <span class='negative_1'>it<\/span> <span class='positive_1'>seemed<\/span> <span class='positive_1'>a<\/span> <span class='negative_1'>bit<\/span> <span class='positive_1'>tight<\/span>! <span class='positive_1'>so<\/span> <span class='negative_1'>i<\/span> <span class='positive_1'>took<\/span> <span class='negative_1'>it<\/span> <span class='positive_1'>back<\/span> <span class='negative_1'>to<\/span> <span class='positive_1'>the<\/span> <span class='negative_1'>store<\/span> & <span class='positive_1'>ordered<\/span> <span class='positive_1'>a<\/span> <span class='positive_1'>larger<\/span> <span class='negative_1'>size<\/span>. <span class='positive_1'>for<\/span> <span class='positive_1'>the<\/span> <span class='negative_1'>sale<\/span> <span class='positive_1'>price<\/span> <span class='positive_1'>this<\/span> <span class='positive_1'>is<\/span> <span class='positive_1'>a<\/span> <span class='negative_1'>great<\/span> <span class='positive_1'>top<\/span>. <\/br> <sub>Label predicted: 0 (85.23%)<br/>Explainer fit: 0.97<\/sub> <\/p> <\/div>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

Dari hasil output observasi kedua terprediksi product tersebut recommended dengan probability 96.31% dan nilai explainer fit menunjukkan seberapa baik LIME dalam menginterpretasikan prediksi untuk observasi ini sebesar 0.89 artinya dapat dikatakan cukup akurat. Teks berlabel biru menunjukkan kata tersebut meningkatkan kemungkinan product tersebut untuk direkomendasikan, sedangkan teks berlabel merah berarti bahwa kata tersebut bertentangan/mengurangi kemungkinan product tersebut untuk direkomendasikan.
