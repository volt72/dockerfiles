# Dockerfiles as ImageDefinition

This is a fork of the dockerfiles [Dinosaur Dataset](https://vsoch.github.io/datasets), with the goal
being to test using the [schema.org](https://www.schema.org) Dataset, SoftwareSourceCode, and 
ContainerRecipe (under development) representations of the containers to make them accessible
with Google Search.

<a target="_blank" href="https://camo.githubusercontent.com/d0eb19f161d4795a9c137b9b71c70b008d7c5e8e/68747470733a2f2f76736f63682e6769746875622e696f2f64617461736574732f6173736574732f696d672f61766f6361646f2e706e67"><img src="https://camo.githubusercontent.com/d0eb19f161d4795a9c137b9b71c70b008d7c5e8e/68747470733a2f2f76736f63682e6769746875622e696f2f64617461736574732f6173736574732f696d672f61766f6361646f2e706e67" alt="https://vsoch.github.io/datasets/assets/img/avocado.png" data-canonical-src="https://vsoch.github.io/datasets/assets/img/avocado.png" style="max-width:100%; float:right" width="200px"></a>

For generation of the Dockerfiles, see the [dockerfiles](https://www.github.com/vsoch/dockerfiles) Github repository.

## Generation

The following steps are covered in the [generateLocal.py](https://github.com/openschemas/dockerfiles/blob/master/generateLocal.py) script provided here. The script does the following:

 1. We are interested in a subset of the data, so we choose "python applications" and thus recursively walk through the original dataset folders and remove those that don't have pip, conda, or python in the build recipe.
 2. We use the [schemaorg example](https://github.com/openbases/extract-dockerfile) to generate an ImageDefinition, Dataset, and SoftwareSourceCode example, one for each, along with generating a datastructure with links to each (that we can use later to create some search interface).
 3. To start, the Dataset example is shown on [Github pages](https://openschemas.github.io/dockerfiles/), with each table page corresponing to a catalog of images.

## Cluster Extraction

To extract the `ImageDefinition` we also need to run [container-diff](https://github.com/GoogleContainerTools/container-diff)
on our datasets! We have about 60k, so this would need to be done in a cluster environment. Thus, once we have our subset, specifically a total of 60,827 Dockerfiles, we can extract pip packages on Sherlock. This means the following steps:

### 1. Container-Diff on Sherlock

To do this we use the scripts [1_run_extractSherlock.py](1_run_extractSherlock) and [1_extractSherlock.py](1_extractSherlock.py). When I first wanted to try this, container-diff wasn't suited for a cluster environment because 1. I couldn't control the cache, and it default to my home (which would fill up almost immediately and then lock me out from logging in again) and 2. I couldn't direct the output to file, and couldn't get a nice solution to write the results to file despite my hacky attempts. To solve these problems, I did two pull requests (merged into master, [1](https://github.com/GoogleContainerTools/container-diff/pull/274) and [2](https://github.com/GoogleContainerTools/container-diff/pull/279)) to add the ability to customize the cache and save output to a file. The release with these features hasn't yet been done yet at the time of writing of this README, so in the meantime we need to install container-diff from master like so:

```
export GOPATH=/scratch/users/vsochat/SOFTWARE/GOROOT
cd /scratch/users/vsochat/SOFTWARE/GOROOT/src/github.com/GoogleContainerTools
git clone https://github.com/GoogleContainerTools/container-diff
cd container-diff
go get
make
```

And then the executable is located here.
```
/scratch/users/vsochat/SOFTWARE/GOROOT/src/github.com/GoogleContainerTools/container-diff/out/container-diff
```

Note that when there is a release, you can curl the binary and add to your path (without needing the above).
I wound up moving the (self compiled) executable into my own ~/bin

```
cp /scratch/users/vsochat/SOFTWARE/GOROOT/src/github.com/GoogleContainerTools/container-diff/out/container-diff ~/bin
```

amd then I could use the scripts to submit jobs. At the end, 54,593 of the container-diffs were successfully extracted.
The missing containers were not extractable typically because I couldn't find the container on Docker Hub.

### 2. ImageDefinition Local Generation

I again used scp to copy the container-diff json files to the repository present working directory.
Now I could use [generate.py](generate.py) using the files to generate the final page indices
and Github Pages. I originally created this index in the first step, but now have it done here because
we weren't able to get all metadata for the 60K total images (about 5K were not found between the time I
extracted the Dockerfiles and then came back to this analysis).


