### INSTALLING AND TESTING THE PROGRAMS

###### Elena Flores Callejas September 2021

##### Aim: Install and test all the programs to be used in the analytical pipeline

List of programs:  SignalP v 5.0 (Almagro *et al*. 2019), EffectorP v2.0 (Sperschneider *et al*. 2015), ApoplastP v2.0 (Sperschneider *et al*. 2018), OutCyte-SP (Zhao *et al.* 2019), WolfPsort (Horton *et al*. 2007; https://www.genscript.com/wolf-psort.html), TMHMM v2.0 (Melén *et al.* 2003; http://www.cbs.dtu.dk/services/TMHMM/), Target P v2.0 (Emanuelsson *et al.* 2000), PS-SCAN (versión 1.79), HMMER v.3.3.2 (Cheng 2014), PredictNLS v1.0.20 (parámetros predeterminados, https://rostlab.org/owiki/index.php/PredictNLS), OrthoFinder v2.5.4 (Emms 2020).

```bash
#Installing and testing
#Signalp
#Move to directory bin 
cd /home/Laccaria_MiSSP/bin
#Untar the downloaded file
tar -xzvf signalp-5.0b.Linux.tar.gz
#Test it with any fasta file 
cd signalp-5.0b
bin/signalp -fasta test/euk10.fsa -org euk -format short -prefix euk_10_short
bin/signalp -fasta test/euk10.fsa -org euk -format long -prefix euk_10_long #fasta string:Input file in fasta format;-org string Organism. Archaea: 'arch', Gram-positive: 'gram+', Gram-negative: 'gram-' or Eukarya: 'euk' (default "euk");-format string Output format. 'long' for generating the predictions with plots, 'short' for the predictions without plots. (default "short");  -prefix string Output files prefix. (default "Input file prefix"); -mature Make fasta file with mature sequence.
#Modify the working directory
#cd /home/Laccaria_MiSSP/bin/signalp-5.0b/bin
./signalp -fasta ../test/euk10.fsa -org euk -format short -prefix euk_10_short
```

```bash
#Installing and testing
#EffectorP
#Move to directory bin 
cd /home/Laccaria_MiSSP/bin
#Downloud EffectorP from the github repo
git clone https://github.com/JanaSperschneider/EffectorP-2.0.gi
#Install EMBOOS
cd EffectorP-2.0.1/Scripts
tar xvf emboss-latest.tar.gz
cd EMBOSS-6.5.7/
 ##Install the necessary libraries
sudo apt-get install build-essential
sudo apt install libx11-dev
./configure
make
#Install weka
cd ../
unzip weka-3-8-1.zip
#Change to quast-env in order yo have a working Python version (in this case 3.6.13)
##Test it
python EffectorP.py -i Effector_Testing.fasta

#END. Working fine. 
#Number of proteins that were tested: 17
#Number of predicted effectors: 14
#82.4 percent are predicted to be effectors.
```

```bash
#Installing and testing
#Apoplast
#Move to directory bin 
cd /home/Laccaria_MiSSP/bin
#Downloud ApoplastP from github
git clone https://github.com/JanaSperschneider/ApoplastP.git
#Install EMBOOS
cd ApoplastP/Scripts/
tar xvf emboss-latest.tar.gz
cd EMBOSS-6.5.7/
./configure
make
#Install weka
cd ../
unzip weka-3-8-1.zip
#Change to quast-env in order yo have a working Python version (in this case 3.6.13)
##Install the necesary module directories and test it
pip3 install biopython
python ApoplastP.py -i Testing.fasta

#END. Working fine.
#Number of proteins that were tested: 29
#Number of predicted apoplastic proteins: 26
#89.7 percent are predicted to be apoplastic.
```

```bash
#Installing and testing
#TMHMM v2.0
#Untar the downloaded file through web page + email http://www.cbs.dtu.dk/cgi-bin/nph-sw_request?tmhmm 
tar -xvzf tmhmm-2.0c.Linux.tar.gz 
cd tmhmm-2.0c
#Insert the correct path for perl 5.x in the first line of the scripts bin/tmhmm and bin/tmhmmformat.pl (if not /usr/local/bin/perl) if necesary
whereis perl #copy the path
less -S tmhmm-2.0c/bin/tmhmm           #modify if necessary
less -S tmhmm-2.0c/bin/tmhmmformat.pl
#Make sure you have an executable version of decodeanhmm in the bin directory.
cd bin && ln -s decodeanhmm.Linux_x86_64 decodeanhmm && cd -
#Include the directory containing tmhmm in your path.
PATH="$PATH:/usr/local/bin/tmhmm/bin"
#Read the TMHMM2.0.guide.html.
firefox TMHMM2.0.html 
#Run the program
tmhmm my_sequences.fasta
#or
cat my_sequences.fasta |tmhmm

#END. Working fine.
```

```bash
#Installation and testing
#TargetP v2.0
cd ../
#Untar the downloaded file through web page + email https://services.healthtech.dtu.dk/cgi-bin/sw_request
tar -xvzf targetp-2.0.Linux.tar.gz
#The package folder contains two folders,'bin' and 'lib'. The executable of targetp-2.0 is inside 'bin'. It can be run directly from that folder but it can also be installed globally. To do that, move or copy the executable 'targetp' to your system bin folder ('/usr/local/bin') user bin folder
cp targetp-2.0/bin/targetp /usr/local/bin
#and move or copy the content of 'lib' to  your system lib folder or user lib folder
cp targetp-2.0/lib /usr/local/lib
#It is important to keep this structure so the executable can find the libraries: */bin/targetp */lib/
# Test TargetP on the 10 eukaryotic sequences shipped with the package
cd targetp-2.0/
bin/targetp -fasta test/example.fsa -org non-pl -format short -prefix example_short
bin/targetp -fasta test/example.fsa -org non-pl -format long -prefix example_long
#The output files should be identical to the corresponding files in 'targetp-.0/test
diff -s example_long_sp_Q00972_BCKD_RAT_pred.txt test/example_long_sp_Q00972_BCKD_RAT_pred.txt

#END. Working fine.
```

```bash
#Installing and testing 
#WolfPsort
cd ../
#Download the zip file from https://github.com/fmaguire/WoLFPSort | move it to bin | unzip it
mv ../../Downloads/WoLFPSort-master.zip .
unzip WoLFPSort-master.zip 
#Now "runWolfPsortSummary" should work
cd WoLFPSort-master
/bin/runWolfPsortSummary fungi < ./bin/testQuery.fasta
#The next step is for "runWolfPsortHtmlTables", but can be skipped if you use the --no-psort-classical-prediction.

##Testing
#There is no test suite, but you can see if your results match those found in REGRESSION
```

```bash
#Installing and testing 
#HMMER v.3.3.2
conda install -c bioconda hmmer
#Test any of the available commands
hmmbuild -h #build profile from input multiple alignment
hmmalign -h #make multiple sequence alignment using a profile
hmmsearch -h #search profile against sequence database
hmmscan -h #search sequence against profile database
hmmpress -h #prepare profile database for hmmscan
phmmer -h #search single sequence against sequence database
jackhmmer -h #iteratively search single sequence against database
nhmmer -h #search DNA query against DNA sequence database
nhmmscan -h #DNA sequence against a DNA profile database
hmmfetch -h #retrieve profile(s) from a profile file
hmmstat -h #show summary statistics for a profile file
hmmemit -h #generate (sample) sequences from a profile
hmmlogo -h #produce a conservation logo graphic from a profile
hmmconvert -h #convert between different profile file formats
hmmpgmd -h #search daemon for the hmmer.org website
hmmpgmd_shard -h #sharded search daemon for the hmmer.org website
makehmmerdb -h #prepare an nhmmer binary database
hmmsim -h #collect score distributions on random sequences
alimask -h #column mask to a multiple sequence alignment

#END. Working fine.
```

```bash
#Installing and testing 
#PredictNLS V.1.0.20-4 
#Install predictnls package and read the manual
sudo apt install predictnls
man predictnls
#Try using the command predictnls
predictnls [FILE]
```

```bash
#Installing and testing
#OrthoFinder v.2.5.4
#Download the latest version of OrthoFinder
curl -L -O https://github.com/davidemms/OrthoFinder/releases/latest/download/OrthoFinder.tar.gz
#Untar the package | move to the OrthoFinder directory
tar xzvf OrthoFinder.tar.gz
cd OrthoFinder/
#Request OrthoFinder to print it's help file
./orthofinder -h
#Testing by running OrthoFinder on the Example Dataset 
 ./orthofinder -f ExampleData/
#Check that the directory with the results has been created
ls ExampleData/OrthoFinder/Results_Sep19/

#END. Working fine.
```

```bash
#Installing and testing
#OutCyte v1.0
#Open the server from the browser 
firefox http://www.outcyte.com/
#In method select: OutCyte-SP and submit a Query
```

```bash
#Installing and testing 
#LOCALIZER v1.0.4
#Move to bin directory
cd ../
#Download de tar package from http://localizer.csiro.au/software.html | move it to bin directory | untar the package
mv ../../Downloads/LOCALIZER_1.0.4.tar.gz .
tar xvf LOCALIZER_1.0.4.tar.gz
#Make sure LOCALIZER has the permission to execute | move to LOCALIZER directory
chmod -R 755 LOCALIZER_1.0.4/
cd LOCALIZER_1.0.4/
#Install EMBOSS by switching to Scripts directory, unpack, configure and make
cd Scripts
tar xvf emboss-latest.tar.gz
cd EMBOSS-6.5.7/
./configure
make
#Instal WEKA by unziping the file weka-3-6-12.zip
cd ../
unzip weka-3-6-12.zip 
#Test if LOCALIZER is working 
python LOCALIZER.py -e -i Effector_Testing.fasta
```

```bash
#Installing and testing 
#PS-SCAN (versión 1.79)
#Rodo nota mental, no encuentro este programa... ni su artículo como para ver la referencia  la liga de descarga...
```

