# Lung Cancer Classification using LBP and Chi Squared Distances
## Classifying cancerous lesions and nodules in the lung using histogramic distances

### Abstract
Just in the US alone, lung cancer affects 225 000 people every year, and is a $12 billion cost on the health care industry. Globally, it remains the leading cause of cancer death for both men and women. Thus, early detection becomes vital in successful diagnosis, as well as prevention and survival. 

Existing solutions in terms of detection are essentially observation-based, where doctors observe x-rays and use their judgement in order to diagnose the disease. Contrastingly, the idea investigated throughout this study was the use of machine learning techniques in order to successfully classify an image data set as cancerous/non-cancerous.

The data set consisted of computed tomography (CT) scans of lungs of 20 patients in the DICOM format. In order to isolate the relevant solid nodules and structures in each image, the data set was put through a series of preprocessing steps involving conversion to Hounsfield Units (a measure of radio density), resampling, morphological masks and zero centering/normalization. Segments of the resulting data was then passed through a Local Binary Pattern (LBP) generating algorithm, that performed the necessary texture-based feature extraction and outputted a histogram vector for each image in the data set. These histograms, combined with the provided ground truths for each patients, formed the training set used to train the Chi-squared distance classifier. Using leave-one-out validation, the classifier’s performance was then validated.

### I.	Introduction
Lung cancer is a tumor characterized by uncontrolled cell growth, specifically in tissues of the lung. Since most cases are not curable, only 17% of diagnosed patients in the United States survive 5 years after diagnosis. The result is approximately 8.8 million deaths anually, implying that it’s currently the most common cause of cancer-related death in both men and women. 

With such fatal effects, early detection is extremely important in ensuring the paient’s chance of survival. In fact, early detection and diagnosis is known to double a person’s chance of survival. The state of the art methods of detection and diagnosis are largely observation based. One of the primary steps in diagnosis is performing a chest radiograph, also known as a chest X-ray. This utilizes the transmission of radiation through the patient’s body to obtain a 2-D snapshot of their anatomy. For further observation, a computed tomography (CT) scan can also be performed, which essentially combines computing, actuators and X-ray technology to produce an accurate 3-D model of the lungs. A more recent development in this field is low-dose helical computed tomography (LDCT). This technology is fairly new, and unproven. It’s primary advantage is that it’s able to detect much smaller tumours (the size of a grain of sand), thereby leading to earlier detection and higher survival rates. This comes at the cost of higher false positve rates, which can lead to unnecessary invasive procedures and therefore, serious clinical complications.

If a tumor is found, it then needs to be classified via histological type, which essentially means tissue type. It’s important to ensure that during this process, patients are not exposed to the CT / X-ray scanning for long or frequent time periods, as it can actually spur/increase the growth of the tumour via radiation.
Further tests are then done in order to identify potential secondary tumors, and their location and spread. Following this assessment, the staging is finally performed, where the “stage” of the cancer (the development of the tumour) is determined. Staging levels range from 1 (being the least severe) to 4 (being the most), and the stage essentially determines factors such as probability of survival, type of treatment received etc. Generally, the earlier the detection and diagnosis, the lower the stage of the tumour. 

Difficulties and issues with this approach is that the diagnosis is largely based on the experience and discretion of the individual doctor, and is conflicted with the various biases that they’ve encountered throught their career. If the patient’s condition matches an unseen exception, it has the potentital to be missed/under-diagnosed, thereby heavily impacting the patient’s chance of survival. It is not uncommon for two doctors to differently diagnose the exact same condition. It’s this variability that heavily hinders the existing method of detection.

In contrast to this observation-based approach, this study will investigate the applications of machine learning techniques in order to perform the diagnosis. Using a set of 20 high resolution CT scans provided by the National Cancer Institute, and hosted on Kaggle, the chosen algorithm will be used to classify the tumours in the data set as malignant or not. The advantages with this approach are clear, with advancements in distributed computing and cloud storage, a diagnostic machine learning algorithm would be able to “learn” from the conditions encountered by thousands of doctors across a wide variety of patients. In doing so, the number of unseen/new exceptions encountered are much lower than the judgement of an individual doctor, thereby increasing the accuracy of the resulting diagnosis.

The objective of this investigation is to validate the capabilities and accuracy of machine learning techniques within the space of lung cancer detection. If proven successful, the goal is that the study will help further the use of machine learning technologies within healthcare, decrease the high false posive rate assocaited with current technologies, and contribute towards improving surivval rates for lung cancer patients.

The study is organized in a chronological fashion. First, the choice of the appropriate algorithm and it’s implementation are discussed. This also includes detailed explanations on the pre-processing required to sucessfully analyze the data set. Then, an analysis of the performance of the method, as well as it’s related results are presented. Finally, the study concludes with additional changes and improvements to the existing implementation, as well as considerations on it’s applications and potential variations.

### II.	Material and Methods
Having established the problem, as well as the objectives of this study, it was necessary to determine the procedure that the study was based on. This includes the sources of data (materials) necessary, as well as the chosen algorithm (method) of operation. Each of these components, as well as their related facets are discussed in the section below.

#### A.	Materials: Sources of Data
The primary source of data utilized throughout the study is the dataset provided by the National Cancer Institute, hosted by Kaggle for the annual Data Science Bowl 2017. This datset consists of over a thousand high-resolution low-dose CT scans from high-risk patients, with each CT scan provided in DICOM format. In order to deal with the lack of available computation time/resources, the resulting solution will be trained and tested on a sample subset of this data, noted to be CT scans of 20 patients. As stated earlier, an individual CT scan consists of several hundered axial cross sectional images of the lung and chest cavity, with each image varying slightly in depth. The dataset also includes ground truths for each patient, which is essentially a binary digit indicating cancerous (1) or non-cancerous (0) tumours.

#### B.	Methods: Algorithm Development
This section will elaborate on various facets of the algorithm development process, namely: 1) Choice of machine learning algorithm, 2) Preprocessing steps, 3) Implementation of the algorithm. These facets will be discussed in the order shown above.

##### 1)	Choice of Machine Learning Algorithm
The selection of the appropriate algorithm largely depended on the amount of data available, as well as the access to necessary computational resources. In this case, the major bottleneck to selecting greater data-hungry deep-learning frameworks such as Convolutional Neural Networks (CNNs) was the lack of computational resources for processing. The chosen dataset provided over 1000 high resolution CT scans, with each scan comprising of around 300 images, leading to approximately 300 000 images available for training. However, with an 8GB RAM processing unit, training an algorithm on such a large dataset would be extremely time-consuming, and impractical.

As a result, a 20-patient subset of the data provided by Kaggle was selected as the primary dataset, to be utilized for training as well as testing. Since the problem essentially consist of classifying CT scans between two major labels, cancerous (1) or non-cancerous (0), this indicated either the use of clustering algorithms like K-means and Fuzzy C-means (FCM, classification algorithms like Support Vector Machines (SVM), or distance-based classification with Chi-Squared.
Having noted that the dataset is pre-labelled, implying that the ground truths are also available, the problem was treated as a supervised learning problem and thus a classification algorithm was chosen. By developing a prototype SVM algorithm, poor performance in terms of classification ability was clearly noted. This can be largely attributed to the poor variance in feature vectors, or the size of the input training data set. With the training data set as a design constraint on the experiment, the distance-based classification approach was chosen as a better alternative. Chi-squared distances provide a better classification performance when compared to Euclidean distances because of their ability to provide weights to their inputs. The distance-based approach is also simple, simple to scale for larger data sets, and also very easily modifiable and thus provides a low development cost.

##### 2)	Pre-Processing
As with most image-based classification problems, pre-processing the images to extract regions of relevant information is a major endeavor. In this case, the pre-processing procedure followed a series of steps, as described below:

###### a)	Conversion to Hounsfield Units (HU)
The unit of measurement for CT scans is generally in Hounsfield Units (HU), which is a measure of radio density. Applying a simple linear scaling step on each image of the CT scan allow conversion from the uncalibrated pixel values, to calibrated HU units. Applying this conversion allows for the filtering of irrelevant substances from the images, such as air and bones.

###### b)	Resampling
Each CT scan had varying resolution of sampling, implying that certain scans had for example, 2.5 mm between slices/images, whereas others may have 1.2 mm of space between slices. This variation was likely to cause an issue in the training phase of the algorithm, since there is lack of consistency in terms of number of slices in the data presented. Thus, all the images were resampled to have a set number of 256 slices, thereby eliminating a source of error and ensuring consistency amongst the data.

###### c)	Segmentation
After generating a resampled version of the data, the most important pre-processing step is to segment, and isolate relevant structures inside the lung. Since each CT scan consists of 300-400 images, and only a few of these images are likely to contain the tumour, it’s very important to perform as much isolation as possible to ensure significance of the relevant data. 

This segmentation is performed by thresholding the image at around -320 HU (which allows for only solid matter in the lungs to be captured), and using morphological closing (an image processing algorithm) to capture the largest possible solid connected volumes within the lungs. This can include air pockets, as well as solid nodules and tumors, however the HU units can be utilized to identify air pockets and disregard them. Thus, the final resulting structure is essentially a masked 3D model of the lung, with only the solid structures within the lung remaining in the dataset. 

##### 3)	SVM Implementation
The implementation of the SVM algorithm was performed primarily utilizing the capabilities of the scikit-learn python package. Training data was prepared for the classification process by running a Local Binary Pattern (LBP) algorithm on the preprocessed images, and generating a resulting histogram of textures. LBP essentially performs a binary comparison on each pixel, to it’s neighboring 8 pixels, and in doing so generates an 8-bit number, an integer. This integer therefore represents the given pixel, and the algorithm repeats this process for each pixel value in the image. In this case, pixel values are represented in Hounsfield Units, and thus are a measure of radio density rather than pixel intensity. Thus, when a histogram is formed of the occurrence of all the generated integers, the occurrence of similarly dense structures (nodules and tumors) should be clearly visible.

However, since there were approximately 300 images per CT scan, and tumours were specific to only a select few images, performing an LBP on each image would make the relevant data completely insignificant. Thus, some imtelligent trimming was performed on the data, by evenly cutting each CT scan to 200 slices, and then generating histograms on 25% of these slices, producing a total of 50 LBP-based histograms. 

Thus, two training input vectors could be formed: an X vector that essentially conained the 50 histograms of each CT scan, for a total of 20 CT scans. The Y vector contained an expansion of the 20 ground truths, therfore each ground truth repeated itself 50 times, accounting for a 1:1 relationship between the truth for a patient and the associated slice in their CT scan. These two vectors then were used to train the SVM classifier.

### III.	Discussion and Conclusion
Having implemented the SVM classification aglorithm, it was now possible to train, and test it’s performance. This disccusion will essentially be comprised of three main components: an analysis on the algorithm’s performance, an evaluation on possible future improvements, and a re-visitation of the intial goals of the study, which will conclude the investigation.

#### A.	Validation and Performance
There are a variety of different validation methods that could have been applied, such as K-fold cross validation, leave-one-out etc. Generally, all validation practices are based on the separation of training data, and testing data. The algorithm is trained using a particular segment of data, and then it’s performance is tested on a completely different data segment. In order to avoid biases in the chosen data sets, multiple different segments can be chosen for testing (K segments is K-fold cross validation), and the algorithm is then re-trained and iterated K times. For larger datasets, a 4 or 5 fold cross validation process is generally a good measure of validation.

In contrast, the leave-one-out validation method essentially selects one data point from the segment as the testing data, and trains the algorithm on the remaining n-1 data points. This nested iterative process is repeated for each data point, essentially performing the most exhaustive validation possible. Although the process is extremely heavy on computation and processing time, it is essentially a complete validation process. In this case, due to the small size of the utilized dataset (20 patients, 1000 histograms), a 4 or 5 fold validation did not seem applicable. Rather, the leave-one-out routine was utilized, allowing for a complete, exhaustive validation of the algorithm.

During each iteration of this algorithm, the error between the predicted result (cancerous or not) must be quantized. There are a variety of error quantization methods that can be utilized, in this case the chosen formula was log-loss.Log loss was an appropriate erorr metric since it essentially incorporates the idea of probabalistic confidence. The formula is tied to information theory, where false classifications are penalised. In doing so, it clearly reflects the probablistic nature of the classifcation algorithm, and thus can accurately depict the delta of distributions between the algorithm output and the ground truths. Thus, by minimizing the log loss, the accuracy of the classifier is maximized. With this context, the resulting performance of the algorithm can be analyzed. 

#### B.	Future Improvements
Despite a valid performance metric, the current implementation of the algorithm definitely has a lot of room for improvement, both in terms of material (data sources), as well as method (algorithm implementation). In terms of data sources, there are a variety of identified sources which can improve the accuracy of the algorithm. With access to greater computing power, or the use of Tensorflow for GPU usage, additional sources of data can be incorporated into the study. 

This primarily includes utilizing the entire 60 GB (over a 1000 patient CT scans as opposed to 20) worth of data from the National Cancer Institute. Another source of data is the Lung Image Database Consortium Image Collection (LIDC-IDRI), which provides an additional 1018 cases and over 124 GB of DICOM image data. Having trained a model on larger sources of data such as this should lead to a significant improvement in prediction accuracy, and therefore a reduction in the resulting log loss error.

There are also numerous method improvements, primarily to do with feature extraction, that would have contributed towards the error in the algorithm. A major issue encountered during the development process was the creation of relevant features using LBP. Since the lung data was segmented using a mask, a majority of the resulting images primarily were empty (just contained a few key segments of data that contributed towards the nodules). Thus, LBP’s feature extraction, and the resulting histograms were heavily skewed towards this “emptiness” in the data, and therefore the extracted features were almost insignificant. To counter this, measures like trimming the dataset and computing only 25% of the image data set as histograms were utilized, and although these measure definitely helped, they essentially masked the issue.

Another discrepancy, or possible inaccuracy was the use of LBP itself. LBP is generally known to operate directly on pictures/images, where pixel values are grayscale intensity values. In contrast, the image in this case was composed in Hounsfield Units (HU), and thus reflected radio density rather than pixel intensity. It is yet to be determined, however this discrepancy in application may also have contributed towards the inaccuracy of the predictions.

Finally, the feature-to-sample ratio, which is essentially a decision on the number of features, could also have been a possible source of error. The current size of the LBP histogram (59), although standard, may be diluting the relevant data if the slices are relatively empty otherwise, and contain no other significant texture features.

Therefore, a coherent solution that addresses the feature extraction portion of the algorithm will largely solve the issues mentioned above, and thus improve the accuracy of the SVM algorithm.

#### C.	Conclusion
Having identified room for improvement, a re-visitation of the initial objectives of this study was necessary. The goals of the study, as mentioned earlier, were to show the potential of a reduction in the number of false positives that occur in modern diagnostic settings, and validate the applicability of machine learning in healthcare.

Although the algorithm has significant areas of improvement in terms of implementation, as well as data sources, at it’s current stage the number of false positives looks extremely promising. The inaccuracies in the algorithm seemed largely to be in the detection of cancer, rather than the false/over-detection of it, implying that a similar, refined algorithm would largely decrease the false positives, and resulting clinical complications that occur in today’s healthcare system. It’s important to note that before such an algorithm is implemented, it needs to be validated extensively in both the positive and false positive test cases, and prove strong performance.

Even from a preliminary study such as this, the promise and applicability of machine learning algorithms in healthcare can definitely be observed. With greater access to larger amounts of data, deep-learning frameworks like CNNs also become applicable. A wider variety of available and competing algorithms can then push forward innovation in the space, and in doing so, accelerate the accuracy of resulting diagnoses. Thus, through a thorough implementation of SVM on a small subset of data, the objectives of the resulting investigation can be deemed met, and the overall study, successful. The eventual goal that studies such as this aim to make progress towards, is the successful use of cutting-edge technology to improve the quality of life, and survival rates of lung cancer patients across the world.

### Acknowledgments
This study would not have been possible without the help of Professor Hamid Tizhoosh, from the Systems Design Engineering Department at the University of Waterloo, as well as the help of Shivam Kalra, Masters student at the University of Waterloo and TA for SYDE 522 – Machine Intelligence.
