# Elastic-ORCID-Prototype
Elastic ORCID Prototype

Put together a prototype the demos how much easier doing ad-hoc queries would be with elastic search.

### References 

* [Quickstart](http://www.elasticsearchtutorial.com/elasticsearch-in-5-minutes.html#Install)


### Setup


*  Open a terminal and cd to project

        cd ~/git/Elastic-ORCID-Prototype

* Start ElasticSearch

        ./elasticsearch-1.5.2/bin/elasticsearch

note: to stop `control-c` to verify it's running you can open `http://localhost:9200` in a browser.

* Open a second terminal

* Cd to project

        cd ~/git/Elastic-ORCID-Prototype
        
* Clean solr instance and populate with sample data


        curl -XDELETE http://localhost:9200/_all
        curl -XPUT http://localhost:9200/record/orcid_v1.2/0000-0001-5109-3700   --upload-file sample_profiles/0000-0001-5109-3700.orcid.json
        curl -XPUT http://localhost:9200/record/orcid_v1.2/0000-0001-6622-4910   --upload-file sample_profiles/0000-0001-6622-4910.orcid.json
        curl -XPUT http://localhost:9200/record/orcid_v1.2/0000-0001-7234-3684   --upload-file sample_profiles/0000-0001-7234-3684.orcid.json
        curl -XPUT http://localhost:9200/record/orcid_v1.2/0000-0002-0036-9460   --upload-file sample_profiles/0000-0002-0036-9460.orcid.json
        curl -XPUT http://localhost:9200/record/orcid_v1.2/0000-0002-1982-1816   --upload-file sample_profiles/0000-0002-1982-1816.orcid.json
        curl -XPUT http://localhost:9200/record/orcid_v1.2/0000-0002-3183-6594   --upload-file sample_profiles/0000-0002-3183-6594.orcid.json
        curl -XPUT http://localhost:9200/record/orcid_v1.2/0000-0002-3219-1820   --upload-file sample_profiles/0000-0002-3219-1820.orcid.json
        curl -XPUT http://localhost:9200/record/orcid_v1.2/0000-0002-6951-5585   --upload-file sample_profiles/0000-0002-6951-5585.orcid.json
        curl -XPUT http://localhost:9200/record/orcid_v1.2/0000-0002-8689-4935   --upload-file sample_profiles/0000-0002-8689-4935.orcid.json
        curl -XPUT http://localhost:9200/record/orcid_v1.2/0000-0003-2161-3781   --upload-file sample_profiles/0000-0003-2161-3781.orcid.json
        curl -XPUT http://localhost:9200/record/orcid_v1.2/0000-0003-3188-6273   --upload-file sample_profiles/0000-0003-3188-6273.orcid.json
        curl -XPUT http://localhost:9200/record/orcid_v1.2/0000-0003-3600-9288   --upload-file sample_profiles/0000-0003-3600-9288.orcid.json
        curl -XPUT http://localhost:9200/record/orcid_v1.2/0000-0003-4654-1403   --upload-file sample_profiles/0000-0003-4654-1403.orcid.json

* finally inspect the index model ElasticSearch created

        curl  http://localhost:9200/record?pretty
        
### Sample queries (and proof this is easier)

