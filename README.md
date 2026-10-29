Test NextFlow Pipeline for AWS S3/EC2/Batch testing
-
John Ma, PhD

Introduction
--
This repo is based on [the tutorial](https://nonstopio.com/blogs/optimizing-computational-workflows-running-nextflow-on-aws-batch/) by Nonstop; however, I performed some syntax cleanup, plus moving the `params` block from `main.nf` to `nextflow.config` following style recommendations.

The pipeline is extremely simple: It contains `FASTQC`, a wrapper for the standard FASTQ QC program [FastQC](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/), and `MULTIQC`, a wrapper for the QC report aggregator [MultiQC](https://seqera.io/multiqc/).

Parameter
__
* **reads**: The location of a FASTQ file. URI is completely acceptable and thus can be used in the this setup.
* **outdir**: The location of the output directory. Like `reads`, URI can be used. 

Input/Output
--
* **Input**: One FASTQ file.
* **Output**: A MultiQC-formatted QC report for the input file, written to `outdir`.

Deviations from the tutorial
--
The tutorial does not specify the versions of `java` and `NextFlow` to install, so it falls into a version hell at the time I attempted. The package installation commands we changed from
```
sudo dnf install java -y
curl -s https://get.nextflow.io | bash
sudo mv nextflow /usr/local/bin/
sudo chmod +x /usr/local/bin/nextflow
nextflow -version
```
to 
```
**export NXF_VER=24.04.4**
sudo dnf install sudo dnf install java**-21-amazon-corretto.x86_64** -y
curl -s https://get.nextflow.io | bash
sudo mv nextflow /usr/local/bin/
sudo chmod +x /usr/local/bin/nextflow
nextflow -version
```

(differences bolded)
This ensures the Java and NextFlow installed are versions 21 and 24.04.4 respectively. Newer version of NextFlow would require Seqera Platform credentials to use certain AWS related functionalility and would not be able to run the pipeline verbatim.
