1.- Obtener data del la base de datos NCBI y descargar el SRA.  
(sudo apt install sra-toolkit
fasterq-dump --split-files SRRXXXXXX)

ESto es para hacer un scrip y lo evaluan: 
#!/bin/bash
# Ruta del archivo 
input_file="sra_list.txt"

# Iterar 
while IFS= read -r sra; do
    echo "Descargando y procesando $sra..."
   
    # Descargar los archivos SRA 
    prefetch "$sra"
   
    # Convertir el archivo SRA descargado a fastq
    fasterq-dump --split-files "$sra"
   
    # Verificar si la conversión fue exitosa
    if [ $? -eq 0 ]; then
        echo "Descarga y conversión de $sra completada con éxito."
    else
        echo "Hubo un problema al convertir $sra"
    fi

done < "$input_file" 


2.- convertir a los archivos _1.fastqc y _2.fastqc y evaluar la calidad FASTQC

3.- Eliminar los adaptadores de la data cruda usando trimmomatic (Recuerde que si tiene read no pareados debe usar la opcion SE)
trimmomatic PE -threads 8 XX_1.fastq XX_2.fastq XX_1_paired.fastq XX_1_unpaired.fastq XX_2_paired.fastq XX_2_unpaired.fastq ILLUMINACLIP:TruSeq3-PE.fa:2:30:10 LEADING:3 TRAILING:3 SLIDINGWINDOW:4:15 MINLEN:36


4.- Ensamblar los genomas (spades, velvet, unicycer..)
spades.py --phred-offset 33 --careful -t 20 --pe1-1 XX_1_paired.fastq --pe1-2 XX_2_paired.fastq --s1 XX_1_unpaired.fastq --s2 XX_2_unpaired.fastq -o spades_out
 
4.- Analizar la calidad de los ensamblados. (Quast) con referencia
quast.py -t 8 -o calidad_out -r referencia.fasta contig.fasta
5.- Enviar en PDF el reporte del ensamblado.
