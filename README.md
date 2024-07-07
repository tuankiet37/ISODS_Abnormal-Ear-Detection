# Abnormal ear detection

The provided notebook is designed to run seamlessly on Google Colab. The primary objective of this task is to identify whether an ear is wearing an earbud or not. For practical implementation, it is recommended to focus on the `Demo` section to run and predict abnormal ears.

In the `Demo` section, follow these guidelines:

- To run on a set of images, provide the path to your image set in the `data_test` variable within the `Run data test` subsection.

- To run on a video, specify the path to your video in the `capture`variable located in the `Video` subsection.

- If you want to analyze a single image, provide the path to that image in the `img` variable within the `Image` subsection

## Proposed approach
Initially, I isolate the ear from the image ultilizing an ear-landmark prediction model(you can find the Github repository [here](https://github.com/Dryjelly/Face_Ear_Landmark_Detection/tree/main) and my pre-trained model on [ISODS's Hugging Face repo](https://huggingface.co/spaces/isods/AI-Proctoring/tree/main/Tran%20Dinh%20Khoi%20-%20Prohibited%20Behaviors/ear%20landmark%20model)).

Subsequently, the inner and outer regions of the ear are extracted separately from the ear.
<table align="center">
    <tr>
        <td align="center"> Inner region</td>
        <td align="center"> Outer region</td>
    </tr> 
    <tr>
        <td align="center"> <img src="./assets/inner1.png" width="300px"></td>
        <td align="center"> <img src="./assets/outer1.png" width="300px"></td>
    </tr> 
</table>

Eventually, I compute mean value color of each channel (RGB) for each region and subtract them. If it is greater than a specified threshold, then it will be assigned as an abnormal ear. A higher subtracted value indicates a greater difference between the regions.

<table align="center">
    <tr>
        <td align="center"> Channel</td>
        <td align="center"> B</td>
        <td align="center"> G</td>
        <td align="center"> R</td>
    </tr> 
    <tr>
        <td align="center"> Inner region(mean value color)</td>
        <td align="center"> 177.6090479405807</td>
        <td align="center"> 175.63808237677245</td>
        <td align="center"> 200.15259959486832</td>
    </tr> 
    <tr>
        <td align="center"> Outer region(mean value color)</td>
        <td align="center"> 131.74234592445328</td>
        <td align="center"> 137.2572564612326</td>
        <td align="center"> 189.7717693836978</td>
    </tr> 
    <tr>
        <td align="center"> Subtracted value</td>
        <td align="center"> 45.86670201612742</td>
        <td align="center"> 38.380825915539845</td>
        <td align="center"> 10.380830211170519</td>
    </tr> 
</table>

## Demo

https://github.com/isods/aiproctoring/assets/155294635/d3c982a7-4921-440e-95ae-11d5043ab62d


https://github.com/isods/aiproctoring/assets/155294635/d3f19c6a-2e17-41a5-9294-1a8754e2f4dd



## Limitations
- It’s worth noting that this method requires a clear and high-quality image of the ear. Because it is easily affected by the environmental conditions(light,contrast,...).

- Additionally, the model can only predict ear landmarks when the input image contains the upper body and one side of the candidate's ear, as follows:

<table align="center">
    <tr>
        <td align="center"> <img src="./assets/abnormal_ear_1.jpg" width="300px" height="300px"></td>
        <td align="center"> <img src="./assets/abnormal_ear_2.jpg" width="300px" height="300px"></td>
    </tr> 
</table>
