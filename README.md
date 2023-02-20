# Transferring health data sets between health systems testing

## Synthetic test data sets 

`notebooks/Recalibration_Loop_Breast_Cancer.ipynb` uses the canonical breast cancer data set to crate synthetic test data sets that are purposefully poorly calibrated, so that finite samples can be drawn and recalibration can be performed. The use case is for data sets where we have model trained on some training data and want to use it in a new setting (such as a different health system), where drawing new samples is expensive. We want to draw a practical number of samples, as efficiently as possible, in order to test the calibration and discrimination of the training model, and then improve by recalibration until we are satisfied with performance in the new setting.

## Planned: Testing methods of Mishra et al. 2022

[link to paper](http://journals.sagepub.com/doi/10.1177/0272989X211044697)

**Note about dataset**

Measure of clinical utility is the standardized net benefit.

* $ R \equiv $ the risk threshold for recommending an intervention.
* $ \pi $ is the prevalence of the outcome
* $\text{TPR}_R \equiv$ the true positive rate when the model risk threshold is $R$. AKA the sensitivity or the hit rate.
* $\text{FPR}_R \equiv$ the same for the false positive rate. AKA 1 - specificity or the probability of a false alarm.
* The ratio $R/(1-R)$ represents the ratio of benefits and harms of recommending an intervention. In this case I would assume this means the benefits vs. harms of screening a patient.

$$ \text{sNB}_R = \text{TPR}_R - \frac{R}{1-R}\frac{1-\pi}{\pi} \text{FPR}_R$$

The advantage of this measure of clinical utility is that its maximum is 1 when the TPR=1 and FPR=0. It is an "opt-in" formulation that assumes the default should be not treating or screening the patient. Other measures of clinical utility include the net benefit and relative utility.

The behavior of the sNB is that as the risk becomes large, it penalizes a higher false positive rate. As the prevalence becomes higher, it penalizes false positives less. For the rare-disease case, we might expect that the risk is high of non-detection, but that the prevalence is somewhat low. So perhaps we want the threshold $R$ closer to 0 to indicate that the our threshold for screening is low.
