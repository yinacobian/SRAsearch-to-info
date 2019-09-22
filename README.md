# SRAsearch-to-info
Tools to explore SRA Search results

Use SRA search tool: https://www.searchsra.org/

Citation: https://www.searchsra.org/pages/citeus

1. Get an account
2. Upload your genome to search
3. Download the results to your local machine

Download all the results

Go to the results.txt file, then find the line results_url= http://149.165.169.158/results/92cca781-e71a-4e47-82a4-42daf415656/results.zip

wget http://149.165.169.158/results/92cca781-e71a-4e47-82a4-42daf4156565/results.zip

4. Unizip the resuls.zip folder
mkdir SRAsearchresults
mv resuls.zip SRAsearchresults
cd SRAsearchresults
unzip resuls.zip

5. Move all files to a single folder

6. Move inexes to a different folder, or delete them

7. Make a list of IDS 

ls | grep '.bam' | cut -f 1 -d '.' | sort -n | uniq > IDS.txt

8. Get the number of hits per SRA entry

cat IDS.txt | xargs -I {id} sh -c 'echo {id}; samtools view {id}.bam | wc -l' | paste - - > hit_counts.tab

9. Sort by number of hits and delete rows with zero hits

sort -k 2 -nr hit_counts.tab | grep -v '0' > sorted_hit_counts.tab

Get info about each metagenome:

cat IDS_sorted.txt | xargs -I {id} sh -c " echo {id}; curl 'https://eutils.ncbi.nlm.nih.gov/entrez/eutils/efetch.fcgi?db=sra&id={id}' 2> /dev/null | xtract -pattern EXPERIMENT_PACKAGE -block EXPERIMENT -element TITLE" > titles_IDS_sorted.txt

