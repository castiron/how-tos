## Set Up a Solr Container in Docker

Some sites that we set up on our local machines have Solr installed, and sharing a Solr instance across multiple dev environments can cause problems. Why don't you set up your own Solr instance just for you? It's super easy!

### Let's get this done!

1. Log in to Shipyard (credentials are in Lastpass)
1. Determine the highest port number in use in docker.cichq.com. View the last container in the list by clicking the id, and noting the value in "Mapping". Your container's port will be this value incremented by 1.
1. Create a new Container by clicking the "Create" button on the Containers list page.
1. Fill in the following fields for your container:
	* Image: castiron/typo3-solr:1.0.6
	* Description: follow established convention of: your initials, then a slash, then the domain for your local installation, eg: lt/fancywobsite.dev
	* Ports: take the port number from the steps above, and append it with ":8080", like so:  99999:8080
	* Hosts: select "Docker"
1. Once you've created your container, you've just got to hook up your TYPO3 instancei (or whatever) to it. Your configuration for TYPO3 will look a little like this, bug you gotta replace the port with your own.

```
# Solr
plugin.tx_solr.solr.host = docker.cichq.com
plugin.tx_solr.solr.path = /solr/core_en
plugin.tx_solr.solr.scheme = http
plugin.tx_solr.solr.port = 99999

```

### You did it. You rock!
