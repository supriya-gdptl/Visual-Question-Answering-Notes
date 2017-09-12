# Visual Question Answering Notes
*My notes on Visual Question Answering(VQA) papers*

**Visual Question Answering**: 
A task of correctly answering the posed question related to a given image.

For example, given a following image and asked a question "What time of the day the picture was clicked?", the VQA model should correctly answer "afternoon".

![alt text](./mscoco-18802.png)

To answer the above question, the VQA model has to process the image and question simultaneously and learn to do reasoning over both modes of input. That means, VQA model has to be good at image processing task as well as natural language processing task.

---

**This repository holds the notes on Visual Question Answering papers that I found interesting.**

1.  *Show, Ask, Attend, and Answer: A Strong Baseline For Visual Question Answering* [[link to paper](https://arxiv.org/pdf/1704.03162.pdf)]
    * The paper uses stacked attention (similar to the approach proposed in Stacked Attention by Yang et al). The image features extracted from pooling layer of ResNet-152 to maintain the spatial information of the image and are L2 normalized. The question features are extracted from output of the last cell of LSTM.
    * The image feature and question feature are concatenated and two layers of convolution are applied, before it is given to the softmax layer for predicting answers.
    * The experiments show that use of l2-normalization, dropout and soft-attention significantly helps in improving the performance.

1.  *Ask Me Anything: Free-form Visual Question Answering Based on Knowledge from External Sources* [[link to paper](http://www.cv-foundation.org/openaccess/content_cvpr_2016/papers/Wu_Ask_Me_Anything_CVPR_2016_paper.pdf)]
    * The paper uses external knowledge base to answer any free-form questions. VQA model extracts the textual information from image and combines them with question, to generate the answer using LSTM.
    * Pretrained VGG16 was used for image processing, DBpedia was used as external source. The external knowledge is encoded using Doc2Vec. Question vector along with textual information of image given to Encoder-Decoder based LSTM for generating answer.

1.  *Image Question Answering using Convolutional Neural Network with Dynamic Parameter Prediction* [[link to paper](http://www.cv-foundation.org/openaccess/content_cvpr_2016/papers/Noh_Image_Question_Answering_CVPR_2016_paper.pdf)]
    * The VQA model needs to select different kind of information from the image based on the question asked. To incorporate this logic, authors propose a parameter prediction layer, which changes the parameters of image module(penultimate layer of VGG16) dynamically. Input to parameter prediction layer is question vector.

1.   *Stacked Attention Network for Image Question Answering* [[link to paper](http://www.cv-foundation.org/openaccess/content_cvpr_2016/papers/Yang_Stacked_Attention_Networks_CVPR_2016_paper.pdf)]
     * To answer a asked question effectively, a VQA model need to look at the right region/object in an image and extract features from that part of image. This concept of attention is implemented by the authors. Image features were extracted from VGG16 final convolutional layer(unlike usual practice of extracting from fully connected layer before softmax). 
     * Text module, that processes question, is implemented by two methods: LSTM and CNN. In case of CNN, unigram, bigram and trigram filters were used. The attention network combines the image features and question features to narrow down the region of image to most significant one.

1.  *Neural Module Network* [[link to paper](https://arxiv.org/pdf/1511.02799.pdf)]    
    * The paper proposes a method to train different modules (fixed set of neural network models), each of which are capable of doing simpler tasks like finding objects, shifting attention window, etc. Network graph is constructed by using these modules, by stacking them in specific order. The order is decided by the parse tree of question.
    * For example, given an image of a person wearing a tie and asked a question "what color is his tie?" then parse tree would be 'color (tie)' and corresponding neural module network will have *attend* module followed by *classify* module. It means, the model has to first find tie by *attending* to it and then find the color using *classify*. The output of *classify* will be used to predict the answer.

1.  *Multimodal Compact Bilinear Pooling for Visual Question Answering and Visual Grounding*[[link to paper](https://arxiv.org/pdf/1606.01847.pdf)]
    * This paper won the VQA challenge 2016. The authors propose that instead of simple arithmetic operation on image feature vector and question feature vector, taking the outer product would help perform better, because the outer product will capture the interaction between every pair of element of two feature vectors. The paper uses the Bilinear pooling model for calculating outer product along with few more techniques to reduce the computational complexity.
    * Image features were extracted from pool-5 of ResNet152 and L2 normalized. The questions are one-hot encoded then given to embedding layer and LSTM. The output of each LSTM cell is concatenated and used as question feature vectors. These images embedding and question embedding is given as input to Multimodal Compact Bilinear model.

1.  *Dual attention networks for multimodal reasoning and matching* [[link to paper](https://arxiv.org/pdf/1611.00471.pdf)] 
    * While predicting the answer, VQA model proposed in this paper, attend to different parts of the image as well as terms of the question at different time stamps i.e. it employs visual attention and textual attention. This mechanism helps in doing combined reasoning, also in cross-domain information retrieval. The paper proposes two algorithms 'reasoning-DAN' and 'matching-DAN'.
    * The paper uses the output of last pooling layer of ResNet-152 as image features. The LSTM used to learn the question feature vector is bidirectional. The sum of the output of LSTM from each cell state of both forward and backward is used as question feature.

