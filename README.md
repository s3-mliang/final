# Final Alignment 

Go to you lab folder. 
```
  cd lab03-$MYGIT
```
Uncompress proteosomes and put all the protein sequences into a single file 
```
  gunzip proteomes/*.gz
  cat  proteomes/*.faa > allprotein.fas
```
Form a BLAST Search to final potential homologs of my gene
```
  makeblastdb -in allprotein.fas -dbtype prot
```
**BLAST search against database to identify homologs**

Create a directory for my gene
```
  mkdir /home/ec2-user/labs/lab03-$MYGIT/NP_005307.1
  cd /home/ec2-user/labs/lab03-$MYGIT/NP_005307.1
```
```
  ncbi-acc-download -F fasta -m protein NP_005307.1
```
Perform a BLAST search using the query protein
```
  blastp -db ../allprotein.fas -query NP_005307.1.fa -outfmt 0 -max_hsps 1 -out NP_005307.1.blastp.typical.out
```
```
  less NP_005307.1.blastp.typical.out
```
Perform a BLAST search and observe the results using less
```
  blastp -db ../allprotein.fas -query NP_005307.1.fa  -outfmt "6 sseqid pident length mismatch gapopen evalue bitscore pident stitle"  -max_hsps 1 -out globins.blastp.detail.out
```
Look at the total number of hits 
```
  grep -c H.sapiens NP_005307.1.blastp.detail.out
```
Set the e-value to be less than 1e-35
```
  awk '{if ($6< 1e-35)print $1 }' NP_005307.1.blastp.detail.out > NP_005307.1.blastp.detail.filtered.out
```
Use this command for an easier way to count the total number of hits in BLAST results 
```
  wc -l NP_005307.1.blastp.detail.filtered.out
```
Use this command to find the number of paralogs for each species 
```
  grep -o -E "^[A-Z]\.[a-z]+" NP_005307.1.blastp.detail.filtered.out  | sort | uniq -c
```
**Save command history and push into GitHub**
```
  history > lab3.commandhistory.txt
```
```
  cd ~/labs/lab06-$MYGIT

  find . -size +5M | sed 's|^\./||g' | cat >> .gitignore; awk '!NF || !seen[$0]++' .gitignore

  git add .

  git status

  git commit -a -m "Adding all new data files I generated in AWS to the repository."

  git pull --no-edit

  git push 
```

# Final Midpoint Rooted Tree

**Download Notung** 
```
    java -jar ~/tools/Notung-3.0-beta/Notung-3.0-beta.jar --help
```
Go into the created folder for your gene 
```
    cd ~/labs/lab06-$MYGIT/NP_005307.1
```
```
    cp ~/labs/lab05-$MYGIT/NP_005307.1/NP_005307.1.homologs.al.mid.treefile ~/labs/lab06-$MYGIT/NP_005307.1/NP_005307.1.homologs.al.mid.treefile
```
```
    ls ~/labs/lab06-$MYGIT/NP_005307.1/NP_005307.1.homologs.al.mid.treefile  
```
Use the following command to perform the reconcilation. 
```
    java -jar ~/tools/Notung-3.0-beta/Notung-3.0-beta.jar -s ~/labs/lab06-$MYGIT/species.tre -g ~/labs/lab06-$MYGIT/NP_005307.1/NP_005307.1.homologs.al.mid.treefile --reconcile --speciestag prefix --savepng --events --outputdir ~/labs/lab06-$MYGIT/NP_005307.1/
```
```
    less -S NP_005307.1.homologs.al.mid.treefile.reconciled.events.txt
```
View the table using this command
```
    nw_display ~/labs/lab06-$MYGIT/species.tre
```
View the nodes using this command
```
    grep NOTUNG-SPECIES-TREE ~/labs/lab06-$MYGIT/NP_005307.1/NP_005307.1.homologs.al.mid.treefile.reconciled | sed -e "s/^\[&&NOTUNG-SPECIES-TREE//" -e "s/\]/;/" | nw_display -
```
Convert special marked-up newick format into RecPhyloXML
```
    python2.7 ~/tools/recPhyloXML/python/NOTUNGtoRecPhyloXML.py -g ~/labs/lab06-$MYGIT/NP_005307.1/NP_005307.1.homologs.al.mid.treefile.reconciled --include.species
```

View Using thirdking
```
thirdkind -Iie -D 40 -f     ~/labs/lab06-$MYGIT/NP_005307.1/NP_005307.1.homologs.al.mid.treefile.reconciled.xml -o  ~/labs/lab06-$MYGIT/NP_005307.1/NP_005307.1.homologs.al.mid.treefile.reconciled.svg
```

**Save history and commit to GitHub** 
```
    history > lab6.commandhistory.txt

    cd ~/labs/lab06-$MYGIT

    find . -size +5M | sed 's|^\./||g' | cat >> .gitignore; awk '!NF || !seen[$0]++' .gitignore

    git add .

    git status

    git commit -a -m "Adding all new data files I generated in AWS to the repository."

    git pull --no-edit

    git push
```

# Final Reconciliation

Go into labs and make a directory for the gene. 
```
    cd ~/labs

    cd lab08-$MYGIT

     mkdir ~/labs/lab08-$MYGIT/NP_005307.1 && cd ~/labs/lab08-$MYGIT/NP_005307.1

    cp ~/labs/lab05-$MYGIT/NP_005307.1/NP_005307.1.homologs.al.mid.treefile ~/labs/
lab08-$MYGIT/NP_005307.1
```
Make a copy of unaligned sequence. 
```
    sed 's/*//' ~/labs/lab04-$MYGIT/NP_005307.1/NP_005307.1.homologs.fas > ~/labs/l
ab08-$MYGIT/NP_005307.1/NP_005307.1.homologs.fas
```
Run RPS-BLAST
```
    rpsblast -query ~/labs/lab08-$MYGIT/NP_005307.1/NP_005307.1.homologs.fas -db ~/data/Pfam -out ~/labs/lab08-$MYGIT/NP_005307.1/NP_005307.1.rps-blast.out  -outfmt "6 qseqid qlen qstart qend evalue stitle" -evalue .0000000001
```
View the output 
```
    cd NP_005307.1

    less NP_005307.1.rps-blast.out
```
Run the script 
```
    sudo /usr/local/bin/Rscript  --vanilla ~/labs/lab08-$MYGIT/plotTreeAndDomains.r ~/labs/lab08-$MYGIT/NP_005307.1/NP_005307.1.homologs.al.mid.treefile ~/labs/lab08-$MYGIT/NP_005307.1/NP_005307.1.rps-blast.out ~/labs/
lab08-$MYGIT/NP_005307.1/NP_005307.1.tree.rps.pdf
```
View in a spreadsheet program
```
    mlr --inidx --ifs "\t" --opprint  cat ~/labs/lab08-$MYGIT/NP_005307.1/NP_005307.1.rps-blast.out | tail -n +2 | less -S
```

Proteins with more than one annotation 
```
    cut -f 1 ~/labs/lab08-$MYGIT/NP_005307.1/NP_005307.1.rps-blast.out | sort | uniq -c
```
Which pfam domain is most commonly found
```
    cut -f 6 ~/labs/lab08-$MYGIT/NP_005307.1/NP_005307.1.rps-blast.out | sort | uniq -c
```

Which protein has the longest annotated protein domain?
```
    awk '{a=$4-$3;print $1,'\t',a;}' ~/labs/lab08-$MYGIT/NP_005307.1/NP_005307.1.rps-blast.out |  sort  -k2nr
```
Which protein has the best e-value?
```
    cut -f 1,5 -d $'\t' ~/labs/lab08-$MYGIT/NP_005307.1/NP_005307.1.rps-blast.out | sort -k2,2rn -t $'\t'
```

**Save the history and push to GitHub** 
```
    history > lab8.commandhistory.txt

    cd ~/labs/lab06-$MYGIT

    find . -size +5M | sed 's|^\./||g' | cat >> .gitignore; awk '!NF || !seen[$0]++'   .gitignore

    git add .

    git status

    git commit -a -m "Adding all new data files I generated in AWS to the repository."
    
    git pull --no-edit

    git push 
```
