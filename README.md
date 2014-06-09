cartridge-store-asset-type
===============

Installation
============

* Edit <ES_HOME>/repository/deployment/server/jaggeryapps/publisher/config/publisher-tenant.json as below
	- "collections": ["gadgets", "sites", "ebooks", "cartridges"]

* Copy <CARTRIDGE_STORE_ZIP>/MobileAppLifeCycle.xml into <ES_HOME>/repository/deployment/server/jaggeryapps/publisher/config/lifecycles/MobileAppLifeCycle.xml

* Edit <ES_HOME>/repository/deployment/server/jaggeryapps/store/config/store-tenant.json as below
	- "assets": ["gadget", "site", "ebook", "cartridge"],

* Copy <CARTRIDGE_STORE_ZIP>/cartridges directory into <ES_HOME>/repository/deployment/server/jaggeryapps/store/extensions/assets

* Copy <CARTRIDGE_STORE_ZIP>/cartridge.rxt into <ES_HOME>/repository/resources/rxts

* Copy <CARTRIDGE_STORE_ZIP>/cartridge.json into <ES_HOME>/repository/deployment/server/jaggeryapps/publisher/config/ext/cartridge.json


Usage
=====

* Go to https://localhost:9443/publisher
* Login and create Cartriges
* Upload cartridges accorinding to the following format.
    <catrtidge-name>
	|----node.pp (sample[01])
	|----<catrridge-name>.json (sample[02])
	|----<cartridge-artifacts>


* Update the lifecycle states until it gets to published state
* Login to the store at https://localhost:9443/store and check the newly added cartridge


-----------------------------------------------------------------------------------------
sample[01]

	# php cartridge node
	node /php/ inherits base {
	  $docroot = "/var/www/"
	  $syslog="/var/log/apache2/error.log"
	  $samlalias="/var/www/"
	  require java
	  class {'agent':
	    type => 'php',
	  }
	  class {'php':}
	  
	  #install php before agent
	  Class['php'] ~> Class['agent']
	}


-----------------------------------------------------------------------------------------
sample[02]

	{
	    "type":"php",
	    "provider":"apache",
	    "host":"stratos.com",
	    "displayName":"PHP",
	 
	    "description":"PHP Cartridge",
	    "version":"7",
	    "defaultAutoscalingPolicy":"economyPolicy",
	    "multiTenant":"false",
	    "portMapping":[
		{
		    "protocol":"http",
		    "port":"80",
		    "proxyPort":"8280"
		},
		{
		    "protocol":"https",
		    "port":"443",
		    "proxyPort":"8243"
		}
	    ],
	    "iaasProvider":[
		{
		    "type":"openstack",
		    "imageId":"RegionOne/<image_id>",
		    "maxInstanceLimit":"4",
		    "property":[
		    {
		        "name":"instanceType",
		        "value":"RegionOne/2"
		    },
		    {
		        "name":"keyPair",
		        "value":"<key_name>"
		    }
		    ]
		}
	    ],
	    "loadBalancer":{
		"type":"lb",
		"property":{
		    "name":"default.load.balancer",
		    "value":"true"
		}
	    },
	}
