Inden automatisering

1. hent alle tilgængelige filer
   bash GetAvailableRasterFileDownloadsWithRetry_insecure.sh

2. opret dato-mappe
   mkdir <yyyymmdd>

3. flyt de dannede json-filer
   mv AvailableRasterFileDownloads_*_<yyyymmdd>.json <yyyymmdd>

4. find keys i top level
   jq -r 'keys[]' ./<yyyymmdd>/*.json | sort |uniq  > top_level_keys_<yyyymmdd>.txt           

5. find keys under paginationMetadata
   jq -r '.paginationMetadata | keys[]' ./<yyyymmdd>/*.json | sort |uniq > paginationMetadata_keys_<yyyymmdd>.txt           

6. find keys under availableFileDownloads
   jq -r 'paths | [.[0]] + .[2:] | map(tostring) | join(".")'  ./<yyyymmdd>/*.json | sort |uniq | grep -v 'paginationMetadata' > availableFileDownloads_keys_<yyyymmdd>.txt
   jq -r 'paths | [.[0]] + .[2:] | map(tostring) | join(".")'  ./20261005/*.json | sort |uniq | grep -v 'paginationMetadata' > availableFileDownloads_keys_20261005.txt

7. Bestem register værdier
   jq -r '.availableFileDownloads[]?.register' ./<yyyymmdd>/*.json | sort |uniq > register_values_<yyyymmdd>.txt

8. Bestem .dataSetName værdier
   jq -r '.availableFileDownloads[]?.dataSetName' ./<yyyymmdd>/*.json | sort |uniq > dataSetName_values_<yyyymmdd>.txt