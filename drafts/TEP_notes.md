# Notes on Tennessee Eastman Process (TEP)

## On the importance of TEP

* https://www.tandfonline.com/doi/full/10.1080/08982112.2018.1461905
* Improvement of continuous processes to stay competetive
* The main challenge of applying SPC and DoE in continuous process settings comes from that these processes are run under engineering process control (EPC).
* DoE involves deliberately disturbing the process to study how the process reacts, and traditionally, this involves studying an important response such as a product quality characteristics or a process output such as the yield.

## Comparison study of data-driven fault diagnosis

* https://www.sciencedirect.com/science/article/pii/S0959152412001503
* The basic data-driven methods, principle component analysis (PCA), partial least squares (PLS), independent component analysis (ICA), fisher discriminant analysis (FDA), subspace aided approach (SAP)
* Two metrics used to assess performance
	* Fault Detection Rate
	* False Alarm Rate
* Standard PCA, which has not considered the autocorrelation of process variable, shows relatively lower FDRs compared with DPCA. Two variants of PLS, i.e. TPLS and MPLS, offer much better FDRs and more accurate fault diagnosis information compared with the standard approach.


## Parallel Autoassociative Neural Networks

* https://www.mdpi.com/2227-9717/7/7/411
* First, although deep learning has been used for process monitoring extensively, in the majority of the existing works, the neural networks were trained in a supervised manner assuming that the normal/fault labels were available. However, this is not always the case in real applications.
* Typical examples of such techniques include principal component analysis (PCA) [3,4,5,6], partial least squares [7,8,9], independent component analysis [10,11], and support vector machine [12,13].
* Check out: A nonlinear support vector machine-based feature selection approach for fault detection and diagnosis: Application to the Tennessee Eastman process. AIChE J. 2019, 65, 992–1005.
* In designing process monitoring systems in a supervised manner, various types of neural networks have been used, such as feedforward neural networks [14], deep belief networks [15], convolutional neural networks [16], and recurrent neural networks [17].
There already exist a few studies where the process monitoring systems for this process are designed on the basis of autoassociative neural networks [23,24,25].

Resources to check out:

* Heo, S.; Lee, J.H. Fault detection and classification using artificial neural networks. IFAC–PapersOnLine 2018, 51, 470–475. [Google Scholar] [CrossRef]
* Zhang, Z.; Zhao, J. A deep belief network based fault diagnosis model for complex chemical processes. Comput. Chem. Eng. 2017, 107, 395–407. [Google Scholar] [CrossRef]
* Wu, H.; Zhao, J. Deep convolutional neural network model based chemical process fault diagnosis. Comput. Chem. Eng. 2018, 115, 185–197. [Google Scholar] [CrossRef]
Choi, E.; Schuetz, A.; Stewart, W.F.; Sun, J. Using recurrent neural network models for early detection of heart failure onset. J. Am. Med. Assoc. 2017, 24, 361–370. [Google Scholar] [CrossRef]
* Yan, W.; Guo, P.; Gong, L.; Li, Z. Nonlinear and robust statistical process monitoring based on variant autoencoders. Chemom. Intell. Lab. 2016, 158, 31–40. [Google Scholar] [CrossRef]
* Jiang, L.; Ge, Z.; Song, Z. Semi-supervised fault classification based on dynamic sparse stacked auto-encoders model. Chemom. Intell. Lab. 2017, 168, 72–83. [Google Scholar] [CrossRef]
* Zhang, Z.; Jiang, T.; Li, S.; Yang, Y. Automated feature learning for nonlinear process monitoring—An approach using stacked denoising autoencoder and k-nearest neighbor rule. J. Process Control 2018, 64, 49–61. [Google Scholar] [CrossRef]