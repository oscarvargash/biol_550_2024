# Week ten: coalescent analysis using Astral

To start this tutorial you need to be logged in the Linux virtual machine
[vlinux.humboldt.edu](https://vlinux.humboldt.edu/)

Once logged in the Linux machine, look for the Terminal, it is an icon that contains the characters `>\_`

You can also write `terminal` in the search bar of the main manu located in the left bottom of the operating system.




## creating a coalescent-consistent tree

> Add the flag to corner of your screen ![](img/yellow.jpeg)

Let's download the data needed for this week's tutorial

```
cd Documents
mkdir week_10
cd week_10
wget https://github.com/oscarvargash/biol_550_2024/raw/main/week_10/files/files.zip
unzip files.zip
```

## Building species trees

### Preparing an analysis file using Beauti

> Add the flag to corner of your screen ![](img/yellow.jpeg)

In order to perform a Bayesian analysis with Beast we need to create a `.xml` file that contains all the information necessary for the analysis. Open Beauti:

```
beauti
```

You will see a window:

1. Drag and drop both fasta files
![](img/beauti.png)
2. Unlink the substitition models by clicking on <kbd>Unlink Subst. Models</kbd> while having both regions being selected
![](img/partition.png)
3. Move to the <kbd>Taxa</kbd> tab and create a taxon set by clicking on <kbd>+</kbd>. Include all the taxa with the exception of Barringtonia. Make this group mononophyletic and add the age of 46. This is the node that will be calibrated to add time to the phylogeny.
![](img/taxa.png)
4. Move to the <kbd>Sites</kbd> tab
![](img/model.png)
5. Select <kbd>GTR</kbd> and <kbd>Gamma</kbd> for each partition
6. Move to the <kbd>Clocks</kbd> tab and select a <kbd>Uncorrelated relaxed clock</kbd>, this models assumes that your tree branches are heterogeneous, a common pattern in most topologies.
![](img/clock.png)
7. Move to the <kbd>Trees</kbd> tab and select a <kbd>Speciation: Yule model</kbd>, this is a simple model of speciation that assumens no significant chages of speciation rate along your tree.
8. Move to the <kbd>Piors</kbd> tab, click on <kbd>* Using Tree Prior</kbd> in front of `tmrca(untitled0)`; this is the prior for the age of the most recent common ancestor for the taxon set we created in step 3. Because this age was inferred from another publication (secondary calibration), we will use a <kbd>Normal</kbd> distribution with a mean of 46 and standard deviation of 5, which roughly correspond to the same interval found in the paper we are using for this calibration.
![](img/prior.png)
9. Go to the <kbd>MCMC</kbd> tab and change the file name stem to: `two_cp_g`. You can see that in this tab you can change all parameters for the MCMC chain.
10. File go to <kbd>file</kbd> and select <kbd>Generate Beast File</kbd>. Save your file in the week_09 folder.

> Remove your flag if you are good to continue ![](img/green.jpeg)

### Runing beast

> Add the flag to corner of your screen ![](img/yellow.jpeg)

You can open beast and run the file we just created. However, we will run this analysis in the CIPRES portal. This is an excelent and free resource to access high computational power for large datasets.

1. Create an account in https://www.phylo.org/
2. Create a new folder `Lecy`
3. Upload the file `two_cp_g.xml` to the subfolder `data`, you can add the label `two_cp_g`
4. Go to <kbd>Tasks</kbd> and crate a new task
5. Add `beast cp` as the description, and select as the <kbd>Imput data</kbd> the `two_cp_g.xml` that you uploaded in the previous step.
6. In the <kbd>Select tool</kbd> tab, select the `BEAST (current) on ACCESS`
7. Click on parameters and change the maximum numbers of hours to run to `12`
8. Finally click <kbd>Save and run</kbd> to run the task 
9. You can check the status of the job in the <kbd>Tasks</kbd>
10. Download results when analysis has finished

> Remove your flag if you are good to continue ![](img/green.jpeg)

### Calculating a chronogram

> Add the flag to corner of your screen ![](img/yellow.jpeg)

To avoid the wait, we will dowload the results from an analysis previously ran:

```
cd ~/Documents/week_09/
wget https://github.com/oscarvargash/biol_550_2024/raw/main/week_09/files/files2.zip
unzip files2.zip
```

Open tracer and load the `*.log` file

```
tracer
```
Does it look like a good analysis?

Look at the `stdout.txt` file in a text editor.

Finally, assuming our analysis is good, we can produce a maximum credibility tree:

```
treeannotator
```

Load the tree file `two_cp_g.trees` as the <kbd>Imput tree file</kbd> using a <kbd>Burnin</kbd> of `5000` trees that correspond to 50%. Name the output `chrono.tre`

```
figtree chrono.tre
```

In figtree show the `scale axis` as `reverse axis`. Also add `node bars` displaying the `height_95_HPD`

Congrats you have crated your first chronogram!


> Remove your flag if you are good to continue ![](img/green.jpeg)

### Exercise

1. Based on the chornogram, what is the most recent divergence in the tree? in other words, which species are the most recent to have diverged? Indicate their age and their confidence interval.

2. What is the age and the confidence interval for of _Bertholletia excelsa_?

