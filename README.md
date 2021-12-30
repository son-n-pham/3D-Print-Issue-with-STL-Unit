# 3D Print Issue with STL Unit When Exporting 3D Model from Siemens Unigraphics NX

This issue seems small but took me awhile to find out the solution. I write it up here for me to review again or for anyone in the world who might face this same problem.

### System:
- Siemen TeamCenter 11
- Unigraphics NX 11

### Objectives:
- Export final model in NX to STL file for inputing into Slicing software, which is Cura in my case.

### Challenges:
- My model is currently in Inch unit (English unit)
- NX does not have on-the-fly selection of units when exporting out to STL, thus the model in inch unit is directly exported out to STL file without unit conversion from inches to mm.
- STL has no unit, and the file is default to understand that the file is already in mm.
- Because of that, the imported STL model from NX into slicing software Cura is incorrectly smaller due to the unit confusion.
- The below has 4 small models which were incorrectly loaded due to the unit issue, and the big was the correct one after the issue was resolved.

  ![image](https://user-images.githubusercontent.com/79841341/147744158-c9db71a3-cded-4ea2-afb3-3c7ec7c90bbe.png)

### Solution:
- In NX, there are options to change units; however, they are just for measurement. When the model is created by inch, that unit is fixed and there is no option to change in NX.
- There is a grip program shared in the below link, which is said to work with NX12. I tried it with my NX11 and it was not working on my case.
  https://community.sw.siemens.com/s/question/0D54O000061xYh1SAE/how-to-change-the-part-unit-from-inch-to-mm-in-nx12
- ug_convert_part.ext is my solution and luckily works. The tool can be found in the below folder, and need to be run in command prompt.

  ![image](https://user-images.githubusercontent.com/79841341/147744748-9552d071-0db4-43b5-9dad-fdd139f9b90c.png)
  
- Another small challenge is to have the NX file in my local folder for conversion. The file is currently store somewhere in the server of Teamcenter. To save the model, right-click on the **model file in Teamcenter**, select **Name References...**, then select the file with the extension of **prj** (The file is usually at the end of the list), and finally **click Download button** to download

![image](https://user-images.githubusercontent.com/79841341/147745120-e443ac14-9907-43ad-be29-41f425091015.png)

![image](https://user-images.githubusercontent.com/79841341/147745601-ebf8fd71-0cbc-4307-8f0e-acf4454126cf.png)

- After having the prj file downloaded from Teamcenter, we just save it to somewhere in the local drive, then open command prompt to run the ug_convert_part tool. In my case, I placed my prj file in development folder in C drive and the tool is in the address C:\Siemens\NX11.0\NXBIN\ug_convert_part.exe. So my syntax in Command Prompt is:

```cmd
# This is to go to the folder c:\development
cd c:\development

# Then run the command to convert
c:\development>C:\Siemens\NX11.0\NXBIN\ug_convert_part.exe -mm <my_file_name>.prt
```

![image](https://user-images.githubusercontent.com/79841341/147746193-12970982-6857-43c1-99ad-df0c671be254.png)

- Now I can use NX to open that converted prj file, then export out to STL. **THE PROBLEM IS RESOLVED!!!**

![image](https://user-images.githubusercontent.com/79841341/147746296-c19eba17-4768-42d2-97b4-86d8d7dee109.png)








