Search Workshop

1. All of the exercises are organized from Jupyter notebooks.

2. The students can load up a notebook and press shift+enter to move onto next cell. Each cell will be either a markdown (text description) or a CQL command.

3. Before executing the notebook, make sure on the top right corner, the CQL kernel is showing as connected and ready to work.

4. The default installed CQL kernel cannot work with DSE-5.0 EAP, so we will have to re-deploy from SteveL's github repo under cstar3 branch. Here are the steps:

```
git clone https://github.com/slowenthal/cql_kernel/
cd cql_kernel
git checkout cstar3
python install.py node0
python setup.py install
```

5. Need to upgrade the python driver to 3.x so that it can work with DSE-5.0 EAP. Matt's original populate.py script only works with DSE-4.x.
apt-get install libev4 libev-dev
pip install --upgrade cassandra-driver (this will install 3.3.0 as of May 4, 2016)

6. Need to load the data initially.
python populate.py metadata_10k.json

Also
python populate_simple.py metadata_10k.json geodata.csv

7. Matt made a mistake on Lab 3, #4 (unless he did it on purpose) "Using CQL search for "Maltese" in the title, sort by title." According to Solr doc, "Sorting can be done on the "score" of the document, or on any multiValued="false" indexed="true" field provided that field is either non-tokenized (ie: has no Analyzer) or uses an Analyzer that only produces a single Term (ie: uses the KeywordTokenizer)". We should leverage this as a confusing exercise for the students and let them find out the hard way. :evilburn:

8. 



#Docker

install docker https://docs.docker.com/engine/installation/linux

add your user to the docker group 

    sudo gpasswd -a ${USER} docker

and refresh:

    newgrp docker

Set up config.txt with your broadcast rpc address (client address for DSE)

    cat /etc/dse/cassandra/cassandra.yaml | grep broadcast_rpc_address:|awk -F' ' '{print $2}' > config.txt



    docker build -t cql-notebook-image .
    docker run --net=host -d -p 0.0.0.0:7001:7001 --name cql-notebook cql-notebook-image


One liner to remove answers from Notebooks:
```
cat Lab\ 1\ Workbook.ipynb |  jq '{"cells":[  .cells[] | select(.cell_type=="raw" // .cell_type=="code" | not) ]} + del (."cells")' > Lab\ 1\ Workbook\ Student.ipynb
```

Or just run `./generate_student_workbooks.sh`
