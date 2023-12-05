# Final Alignment 

cd lab03-$MYGIT

gunzip proteomes/*.gz

cat  proteomes/*.faa > allprotein.fas

makeblastdb -in allprotein.fas -dbtype prot

mkdir /home/ec2-user/labs/lab03-$MYGIT/NP_005307.1

cd /home/ec2-user/labs/lab03-$MYGIT/NP_005307.1

ncbi-acc-download -F fasta -m protein NP_005307.1

blastp -db ../allprotein.fas -query NP_005307.1.fa -outfmt 0 -max_hsps 1 -out NP_005307.1.blastp.typical.out

less NP_005307.1.blastp.typical.out

blastp -db ../allprotein.fas -query NP_005307.1.fa  -outfmt "6 sseqid pident length mismatch gapopen evalue bitscore pident stitle"  -max_hsps 1 -out globins.blastp.detail.out

grep -c H.sapiens NP_005307.1.blastp.detail.out

awk '{if ($6< 1e-35)print $1 }' NP_005307.1.blastp.detail.out > NP_005307.1.blastp.detail.filtered.out

wc -l NP_005307.1.blastp.detail.filtered.out

grep -o -E "^[A-Z]\.[a-z]+" NP_005307.1.blastp.detail.filtered.out  | sort | uniq -c

history > lab3.commandhistory.txt

cd ~/labs/lab06-$MYGIT

find . -size +5M | sed 's|^\./||g' | cat >> .gitignore; awk '!NF || !seen[$0]++' .gitignore

git add .

git status

git commit -a -m "Adding all new data files I generated in AWS to the repository."

git pull --no-edit

git push 

# Final Midpoint Rooted Tree

cd ~/labs/lab06-$MYGIT/NP_005307.1

cp ~/labs/lab05-$MYGIT/NP_005307.1/NP_005307.1.homologs.al.mid.treefile ~/labs/lab06-$MYGIT/NP_005307.1/NP_005307.1.homologs.al.mid.treefile

ls ~/labs/lab06-$MYGIT/NP_005307.1/NP_005307.1.homologs.al.mid.treefile  

java -jar ~/tools/Notung-3.0-beta/Notung-3.0-beta.jar -s ~/labs/lab06-$MYGIT/species.tre -g ~/labs/lab06-$MYGIT/NP_005307.1/NP_005307.1.homologs.al.mid.treefile --reconcile --speciestag prefix --savepng --events --outputdir ~/labs/lab06-$MYGIT/NP_005307.1/

less -S NP_005307.1.homologs.al.mid.treefile.reconciled.events.txt

nw_display ~/labs/lab06-$MYGIT/species.tre

grep NOTUNG-SPECIES-TREE ~/labs/lab06-$MYGIT/NP_005307.1/NP_005307.1.homologs.al.mid.treefile.reconciled | sed -e "s/^\[&&NOTUNG-SPECIES-TREE//" -e "s/\]/;/" | nw_display -

python2.7 ~/tools/recPhyloXML/python/NOTUNGtoRecPhyloXML.py -g ~/labs/lab06-$MYGIT/NP_005307.1/NP_005307.1.homologs.al.mid.treefile.reconciled --include.species

thirdkind -Iie -D 40 -f ~/labs/lab06-$MYGIT/NP_005307.1/NP_005307.1.homologs.al.mid.treefile.reconciled.xml -o  ~/labs/lab06-$MYGIT/NP_005307.1/NP_005307.1.homologs.al.mid.treefile.reconciled.svg

history > lab6.commandhistory.txt

cd ~/labs/lab06-$MYGIT

find . -size +5M | sed 's|^\./||g' | cat >> .gitignore; awk '!NF || !seen[$0]++' .gitignore

git add .

git status

git commit -a -m "Adding all new data files I generated in AWS to the repository."

git pull --no-edit

git push 

# Final Reconciliation

cd ~/labs

 cd lab08-$MYGIT

cp ~/labs/lab05-$MYGIT/NP_005307.1/NP_005307.1.homologs.al.mid.treefile ~/labs/
lab08-$MYGIT/NP_005307.1

sed 's/*//' ~/labs/lab04-$MYGIT/NP_005307.1/NP_005307.1.homologs.fas > ~/labs/l
ab08-$MYGIT/NP_005307.1/NP_005307.1.homologs.fas

rpsblast -query ~/labs/lab08-$MYGIT/NP_005307.1/NP_005307.1.homologs.fas -db ~/data/Pfam -out ~/labs/lab08-$MYGIT/NP_005307.1/NP_005307.1.rps-blast.out  -outfmt "6 qseqid qlen qstart qend evalue stitle" -evalue .0000000001

cd NP_005307.1

less NP_005307.1.rps-blast.out

sudo /usr/local/bin/Rscript  --vanilla ~/labs/lab08-$MYGIT/plotTreeAndDomains.r ~/labs/lab08-$MYGIT/NP_005307.1/NP_005307.1.homologs.al.mid.treefile ~/labs/lab08-$MYGIT/NP_005307.1/NP_005307.1.rps-blast.out ~/labs/
lab08-$MYGIT/NP_005307.1/NP_005307.1.tree.rps.pdf


mlr --inidx --ifs "\t" --opprint  cat ~/labs/lab08-$MYGIT/NP_005307.1/NP_005307.1.rps-blast.out | tail -n +2 | less -S

cut -f 1 ~/labs/lab08-$MYGIT/NP_005307.1/NP_005307.1.rps-blast.out | sort | uniq -c

cut -f 6 ~/labs/lab08-$MYGIT/NP_005307.1/NP_005307.1.rps-blast.out | sort | uniq -c

awk '{a=$4-$3;print $1,'\t',a;}' ~/labs/lab08-$MYGIT/NP_005307.1/NP_005307.1.rps-blast.out |  sort  -k2nr

cut -f 1,5 -d $'\t' ~/labs/lab08-$MYGIT/NP_005307.1/NP_005307.1.rps-blast.out | sort -k2,2rn -t $'\t' 

history > lab8.commandhistory.txt

cd ~/labs/lab06-$MYGIT

find . -size +5M | sed 's|^\./||g' | cat >> .gitignore; awk '!NF || !seen[$0]++' .gitignore

git add .

git status

git commit -a -m "Adding all new data files I generated in AWS to the repository."

git pull --no-edit

git push 
