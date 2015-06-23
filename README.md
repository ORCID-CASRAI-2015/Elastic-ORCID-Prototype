# Elastic-ORCID-Prototype
Elastic ORCID Prototype

Put together a prototype the demos how much easier doing ad-hoc queries would be with elastic search.

### References 

* [Quickstart](http://www.elasticsearchtutorial.com/elasticsearch-in-5-minutes.html#Install)


### Setup

*  Verify your java version is >= 1.7

        java -version

*  Open a terminal and cd to project

        cd ~/git/Elastic-ORCID-Prototype

* Start ElasticSearch

        ./elasticsearch-1.5.2/bin/elasticsearch

note: to stop `control-c` to verify it's running you can open `http://localhost:9200` in a browser.

* Open a second terminal

* Cd to project

        cd ~/git/Elastic-ORCID-Prototype
        
* Clean solr instance and populate with sample data

        # Delete existing index(we will use index names 'record' for Orcid ver 1.2x and 'record_v2.0' for Orcid ver 2.0x)
        curl -XDELETE http://localhost:9200/record
		curl -XDELETE http://localhost:9200/record_v2.0

        # Upload mappings for indexes
        curl -XPUT http://localhost:9200/record --upload-file sample_profiles/mapping.json
		curl -XPUT http://localhost:9200/record_v2.0 --upload-file sample_profiles/v2.0/mapping.json

        # Add json documents		
        curl -XPUT http://localhost:9200/record/orcid_v1.2/0000-0001-5109-3700 --upload-file sample_profiles/0000-0001-5109-3700.orcid.json
        curl -XPUT http://localhost:9200/record/orcid_v1.2/0000-0001-6622-4910 --upload-file sample_profiles/0000-0001-6622-4910.orcid.json
        curl -XPUT http://localhost:9200/record/orcid_v1.2/0000-0001-7234-3684 --upload-file sample_profiles/0000-0001-7234-3684.orcid.json
        curl -XPUT http://localhost:9200/record/orcid_v1.2/0000-0002-0036-9460 --upload-file sample_profiles/0000-0002-0036-9460.orcid.json
        curl -XPUT http://localhost:9200/record/orcid_v1.2/0000-0002-1982-1816 --upload-file sample_profiles/0000-0002-1982-1816.orcid.json
        curl -XPUT http://localhost:9200/record/orcid_v1.2/0000-0002-3183-6594 --upload-file sample_profiles/0000-0002-3183-6594.orcid.json
        curl -XPUT http://localhost:9200/record/orcid_v1.2/0000-0002-3219-1820 --upload-file sample_profiles/0000-0002-3219-1820.orcid.json
        curl -XPUT http://localhost:9200/record/orcid_v1.2/0000-0002-6951-5585 --upload-file sample_profiles/0000-0002-6951-5585.orcid.json
        curl -XPUT http://localhost:9200/record/orcid_v1.2/0000-0002-8689-4935 --upload-file sample_profiles/0000-0002-8689-4935.orcid.json
        curl -XPUT http://localhost:9200/record/orcid_v1.2/0000-0003-2161-3781 --upload-file sample_profiles/0000-0003-2161-3781.orcid.json
        curl -XPUT http://localhost:9200/record/orcid_v1.2/0000-0003-3188-6273 --upload-file sample_profiles/0000-0003-3188-6273.orcid.json
        curl -XPUT http://localhost:9200/record/orcid_v1.2/0000-0003-3600-9288 --upload-file sample_profiles/0000-0003-3600-9288.orcid.json
        curl -XPUT http://localhost:9200/record/orcid_v1.2/0000-0003-4654-1403 --upload-file sample_profiles/0000-0003-4654-1403.orcid.json
		
		curl -XPUT http://localhost:9200/record_v2.0/orcid_v2.0/1 --upload-file sample_profiles/v2.0/1.json
		curl -XPUT http://localhost:9200/record_v2.0/orcid_v2.0/2 --upload-file sample_profiles/v2.0/2.json
		curl -XPUT http://localhost:9200/record_v2.0/orcid_v2.0/3 --upload-file sample_profiles/v2.0/3.json
		curl -XPUT http://localhost:9200/record_v2.0/orcid_v2.0/4 --upload-file sample_profiles/v2.0/4.json
		curl -XPUT http://localhost:9200/record_v2.0/orcid_v2.0/5 --upload-file sample_profiles/v2.0/5.json
		curl -XPUT http://localhost:9200/record_v2.0/orcid_v2.0/6 --upload-file sample_profiles/v2.0/6.json
		curl -XPUT http://localhost:9200/record_v2.0/orcid_v2.0/7 --upload-file sample_profiles/v2.0/7.json
		curl -XPUT http://localhost:9200/record_v2.0/orcid_v2.0/8 --upload-file sample_profiles/v2.0/8.json
		
		

* finally inspect the indexes model ElasticSearch created

        curl  http://localhost:9200/record?pretty
		curl  http://localhost:9200/record_v2.0?pretty
        
### Sample queries (and proof this is easier)

* Retrieve record

        curl http://localhost:9200/record/orcid_v1.2/0000-0001-6622-4910
        curl http://localhost:9200/record_v2.0/orcid_v2.0/1

* Simple count

        curl http://localhost:9200/record/_count?pretty
        curl http://localhost:9200/record_v2.0/_count?pretty
        
* Retrieve all 

        curl -XGET 'http://localhost:9200/record/_search?pretty=true' -d '
        {
        "query" : { 
             "match_all" : {} 
           }
        }'

		curl -XGET 'http://localhost:9200/record_v2.0/_search?pretty=true' -d '
        {
       	"query" : { 
             "match_all" : {} 
           }
        }'
        
* Match by a phrase

        curl -XGET 'http://localhost:9200/record/_search?pretty=true' -d '
        { 
           "query" : { 
              "match" : { "orcid-profile.orcid-bio.personal-details.given-names.value":"alice" }
           } 
        }'
		
		curl -XGET 'http://localhost:9200/record_v2.0/_search?pretty=true' -d '
        { 
           "query" : { 
              "match" : { "record.activities.activities-summary.educations.education-summary.department-name":"Neurosciences" }
           } 
        }'


* Match by organization name of affiliation 

        curl -XGET 'http://localhost:9200/record/_search?pretty=true' -d '
        { 
           "query" : { 
               "match" : { "orcid-profile.orcid-activities.affiliations.affiliation.organization.name":"orcid" }
           } 
        }'
		
		curl -XGET 'http://localhost:9200/record_v2.0/_search?pretty=true' -d '
        { 
           "query" : { 
               "match" : { "record.activities.activities-summary.educations.education-summary.organization.name":"Stanford" }
           } 
        }'


note: Work Identifier has been added as a multifield, so that it is possible to make an exact, as well as, a fuzzy match on it. With type multifield, the work identifier is being stored in 2 different fields, one default(with the name 'value' or 'external-identifier-id') and the other with non-analyzed index(with the name 'value-exact' or 'external-identifier-id-exact'). The insert however is only made for the default index. ElasticSearch takes care of the rest(Check mapping and input files for reference).

* Match by work identifier exact value 

        # To make an exact match, the 'term' keyword should be used. Also notice the use of 'value-exact'
        curl -XGET 'http://localhost:9200/record/_search?pretty=true' -d '
        {
        "query" : {
            "term" : {
            "orcid-profile.orcid-activities.orcid-works.orcid-work.work-external-identifiers.work-external-identifier.work-external-identifier-id.value.value-exact" : "US 20120209644 A1"
        }
        }}'

        curl -XGET 'http://localhost:9200/record_v2.0/_search?pretty=true' -d '
        {
        "query" : {
            "term" : {
            "record.activities.activities-summary.works.group.identifiers.identifier.external-identifier-id.external-identifier-id-exact" : "Test External Identifier Id 0"
        }
        }}'


  
        
* Match by work identifier fuzzy value 
	
        # To make a fuzzy match, the 'match' keyword should be used.
        curl -XGET 'http://localhost:9200/record/_search?pretty=true' -d '
        {
 	    "query" : {
	        "match" : {
		    "orcid-profile.orcid-activities.orcid-works.orcid-work.work-external-identifiers.work-external-identifier.work-external-identifier-id.value" : "US 20120209644 A1"
		}
	    }}'
		
		
        curl -XGET 'http://localhost:9200/record_v2.0/_search?pretty=true' -d '
        {
        "query" : {
            "match" : {
            "record.activities.activities-summary.works.group.identifiers.identifier.external-identifier-id" : "Test External Identifier Id"
        }
        }}'



* Match by work identifier value and type

        curl -XGET 'http://localhost:9200/record/_search?pretty=true' -d '
        { 
        "query" : { 
           "bool" : { 
            	"should": [
              	{ "match_phrase": { "orcid-profile.orcid-activities.orcid-works.orcid-work.work-external-identifiers.work-external-identifier.work-external-identifier-type":"PAT" }},
              	{ "match_phrase": { "orcid-profile.orcid-activities.orcid-works.orcid-work.work-external-identifiers.work-external-identifier.work-external-identifier-id.value":"US 20120209644 A1"}}
            	],
            	"minimum_should_match": 2}
        	}
        }'
		
		curl -XGET 'http://localhost:9200/record_v2.0/_search?pretty=true' -d '
        { 
        "query" : { 
           "bool" : { 
                "should": [
                { "match_phrase": { "record.activities.activities-summary.works.group.identifiers.identifier.external-identifier-id":"Test External Identifier Id 0" }},
                { "match_phrase": { "record.activities.activities-summary.works.group.identifiers.identifier.external-identifier-type":"DOI"}}
                ],
                "minimum_should_match": 2}
            }
        }'
 

* Count (_count) match by work identifier value and type

        curl -XGET 'http://localhost:9200/record/_count?pretty=true' -d '
        { 
        "query" : { 
           "bool" : { 
            	"should": [
              	{ "match": { "orcid-profile.orcid-activities.orcid-works.orcid-work.work-external-identifiers.work-external-identifier.work-external-identifier-type":"PAT" }},
              	{ "match": { "orcid-profile.orcid-activities.orcid-works.orcid-work.work-external-identifiers.work-external-identifier.work-external-identifier-id.value":"US 20120209644 A1"}}
            	]}
        	}
        }'
		
		curl -XGET 'http://localhost:9200/record_v2.0/_count?pretty=true' -d '
        { 
        "query" : { 
           "bool" : { 
                "should": [
                { "match_phrase": { "record.activities.activities-summary.works.group.identifiers.identifier.external-identifier-id":"Test External Identifier Id 0" }},
                { "match_phrase": { "record.activities.activities-summary.works.group.identifiers.identifier.external-identifier-type":"DOI"}}
                ],
                "minimum_should_match": 2}
            }
        }'
		
### Percolator

* Register queries in the percolator
		
        curl -XPUT 'localhost:9200/record_v2.0/.percolator/matchEduOrgQuery' -d '
        {
        "query" : {
            "match" : {
                "record.activities.activities-summary.educations.education-summary.organization.name" : "Stanford"}
            }
        }'
		
        curl -XPUT 'localhost:9200/record_v2.0/.percolator/matchEduDepQuery' -d '
        {
        "query" : {
            "match" : {
                "record.activities.activities-summary.educations.education-summary.department-name":"Neurosciences"}
            }
        }'
		
		
note: Best practice is to create a separate index for all the queries, but requires the type-mapping to be available in the index.
		
* Match a document to the registered queries

        curl -XGET 'localhost:9200/record_v2.0/orcid_v2.0/_percolate' -d '
        {
        "doc" : {
            "record.activities.activities-summary.educations.education-summary.organization.name" : "Stanford university"}
        }'
		
        curl -XGET 'localhost:9200/record_v2.0/orcid_v2.0/_percolate' -d '
        {
        "doc" : {
            "record.activities.activities-summary.educations.education-summary.department-name":"My name is Neurosciences"}
        }'

* Match an existing document to the registered queries

        curl -XGET 'localhost:9200/record_v2.0/orcid_v2.0/1/_percolate'
		
		
note: This returns 2 matches, one for each indexed query.
