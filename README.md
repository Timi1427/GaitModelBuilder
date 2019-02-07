# GaitModelBuilder

This is a library made for creating user profiles and to classify data for the project "LetMeIn" (https://github.com/MilleJanos/GaitBiometrics). This project is under construction. In this part I use the weka library and FeatureExtractor (https://github.com/NemethKrisztian96/FeatureExtractor/) for managing data.

## Installation

Firstly you need a Java compiler, for compiling the .java files from GaitModelBuilder/src/ro/sapientia/gaitbiom/ folder. Or it is easier if you just import in your Java project the GaitModelBuilder.jar.

## Usage

### To build a model
Firstly we need at least two .arff file.  One with positive samples and one with negative samples. Samples are gained from a raw data file with using the Feature Extractor. The raw file contains five columns: a timestamp, x, y, z coordinates of the phone accelerometer, and the number of the step. From this the function get the features, than count a validation rate. These sample files must contains a specific header:

<details>
@relation accelerometer <br>
<br>
@attribute minimum_for_axis_X numeric <br>
@attribute minimum_for_axis_Y numeric <br>
@attribute minimum_for_axis_Z numeric <br>
@attribute minimum_for_magnitude numeric <br>
@attribute average_acceleration_for_axis_X numeric <br>
@attribute average_acceleration_for_axis_Y numeric <br>
@attribute average_acceleration_for_axis_Z numeric <br>
@attribute average_acceleration_for_magnitude numeric <br>
@attribute standard_deviation_for_axis_X numeric <br>
@attribute standard_deviation_for_axis_Y numeric <br>
@attribute standard_deviation_for_axis_Z numeric <br>
@attribute standard_deviation_for_magnitude numeric <br>
@attribute average_absolute_difference_for_axis_X numeric <br>
@attribute average_absolute_difference_for_axis_Y numeric <br>
@attribute average_absolute_difference_for_axis_Z numeric <br>
@attribute average_absolute_difference_for_magnitude numeric <br>
@attribute zero_crossing_rate_for_axis_X numeric <br>
@attribute zero_crossing_rate_for_axis_Y numeric <br>
@attribute zero_crossing_rate_for_axis_Z numeric <br>
@attribute bin0_X numeric <br>
@attribute bin1_X numeric <br>
@attribute bin2_X numeric <br>
@attribute bin3_X numeric <br>
@attribute bin4_X numeric <br>
@attribute bin5_X numeric <br>
@attribute bin6_X numeric <br>
@attribute bin7_X numeric <br>
@attribute bin8_X numeric <br>
@attribute bin9_X numeric <br>
@attribute bin0_Y numeric <br>
@attribute bin1_Y numeric <br>
@attribute bin2_Y numeric <br>
@attribute bin3_Y numeric <br>
@attribute bin4_Y numeric <br>
@attribute bin5_Y numeric <br>
@attribute bin6_Y numeric <br>
@attribute bin7_Y numeric <br>
@attribute bin8_Y numeric <br>
@attribute bin9_Y numeric <br>
@attribute bin0_Z numeric <br>
@attribute bin1_Z numeric <br>
@attribute bin2_Z numeric <br>
@attribute bin3_Z numeric <br>
@attribute bin4_Z numeric <br>
@attribute bin5_Z numeric <br>
@attribute bin6_Z numeric <br>
@attribute bin7_Z numeric <br>
@attribute bin8_Z numeric <br>
@attribute bin9_Z numeric <br>
@attribute bin0_magnitude numeric <br>
@attribute bin1_magnitude numeric <br>
@attribute bin2_magnitude numeric <br>
@attribute bin3_magnitude numeric <br>
@attribute bin4_magnitude numeric <br>
@attribute bin5_magnitude numeric <br>
@attribute bin6_magnitude numeric <br>
@attribute bin7_magnitude numeric <br>
@attribute bin8_magnitude numeric <br>
@attribute bin9_magnitude numeric <br>
@attribute userID {username} <br>
 <br>
@data <br>

</details>

After this we merge these files to get a specific file for binary classifier.
mergeEquallyArffFiles(negativeData, positiveData);
The variable 'negativeData' and 'positiveData' are strings whitch contains the full path of the file. For example: "INPUT_ARFF/features_WsY044SgeaeZtDrQKVpRyWpo7hx1_1.arff".
The merged file is stored in the second parameter's location.

With the CreateAndSaveModel function we can generate a new binary classifier for the user and stored in file system.
The first parameter is the full path of the file, which we get from the previous step, and the second is the file location what we want for storing the model (contains the model file name and type).

### To validate a new raw file with a previously created one

The first step with the merging the specific files is the same as in 'To build a model' section.

Firstly we need a model builder instance.
IGaitModelBuilder builder = new GaitModelBuilder();

Then we create a model from the merged feature file.
Classifier classifier = builder.createModel(FEATURE_USER_FILE);

After this we extract the attributes from file.
ArrayList<Attribute> attributes = builder.getAttributes(FEATURE_USER_FILE);
System.out.println( "NumAttributes: "+attributes.size() );

In the next step we create a new verifier.
IGaitVerification verifier = new GaitVerification();

With the verifier's verifyUser function we can get a number from 0 to 1 which means the probability that the new sample belongs to the user. The TEST_RAW_FILE_USER is a path of the raw file. The raw file are similar properties with the raw file from in the previous section. Then we just print the result.
System.out.println("Probability: " + verifier.verifyUser(classifier, attributes, TEST_RAW_FILE_USER));


## Support

You can open an issue in the Issues section on Github, or contact the project admin at fuloptimea1427@gmail.com.

## Authors and acknowledgment

This project has been made by Fülöp Timea for a research project, under the careful supervision of prof. dr. Antal Margit from Sapientia Hungarian University of Transylvania.

## License
[MIT](https://choosealicense.com/licenses/mit/)
