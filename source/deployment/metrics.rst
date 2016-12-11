Performance Monitoring (E20) - Beta
===============================

*Available in Enterprise Edition E20*.

Performance monitoring support enables a Mattermost server to track system health through integrations with `Prometheus <https://prometheus.io/>`_ and `Grafana <http://grafana.org/>`_, an essential feature for large Enterprise deployments. This documentation provides a deployment guide and ... // XXX add links for section headings



configuration
 - prometheus
 - grafana
 
metrics
 - go metrics --> sample chart
 - mattermost --> sample chart

troubleshooting


Configuration
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Details on integrating your Mattermost server with Prometheus and Grafana.

Installing Prometheus
------------------------------------------------

1 - `Download a precompiled binary for Prometheus <https://prometheus.io/download/>`_. Binaries are provided for many popular distributions, including Darwin, Linux and Windows.

 - For install instructions, see `Prometheus install guides <https://prometheus.io/docs/introduction/getting_started/>`_

2 - The following settings are recommended in the basic Prometheus configuration file named ``prometheus.yml``:
 
.. code:: yaml

    # my global config
    global:
      scrape_interval:     60s # By default, scrape targets every 15 seconds.
      evaluation_interval: 60s # By default, scrape targets every 15 seconds.
      # scrape_timeout is set to the global default (10s).

      # Attach these labels to any time series or alerts when communicating with
      # external systems (federation, remote storage, Alertmanager).
      external_labels:
          monitor: 'mattermost-monitor'

    # Load rules once and periodically evaluate them according to the global 'evaluation_interval'.
    rule_files:
      # - "first.rules"
      # - "second.rules"

    # A scrape configuration containing exactly one endpoint to scrape:
    # Here it's Prometheus itself.
    scrape_configs:
      # The job name is added as a label `job=<job_name>` to any timeseries scraped from this config.
      - job_name: 'prometheus'

        # Override the global default and scrape targets from this job every 5 seconds.
        # scrape_interval: 5s

        # metrics_path defaults to '/metrics'
        # scheme defaults to 'http'.

        static_configs:
          - targets: ["<hostname1>:<port>", "<hostname2>:<port>"]
  }

3 - Enable performance monitoring in the System Console and specify the Listen Address. See more detail in our `configuration settings documentation <>`_. // XXX Link

// XXX Image

4 - To test the server is running, run ``ip:port/metrics``

5 - Run ``vi prometheus.yml`` to finish configuring Prometheus.

// XXX Some notes about configuration

7.	Then they have instructions to starting the service for collecting data
8.	Please use standard docs to have it running
9.	Go to this address localhost:9090/graph
a.	http://metrics.mattermost.com:9090/graph
10.	Sample chart in prometheus


Installing Grafana
------------------------------------------------

12.	Now set up these charts in Grafana
a.	Install from http://docs.grafana.org/installation/debian/
13.	Start the server
a.	Their package installs it as a service
b.	http://metrics.mattermost.com:3000/login admin/admin


METRICS

b.	Go in front of it is standard GO metrics
i.	Geared to users
c.	Those with mattermost added by us
d.	Copy this and paste to docs
e.	Profiling stuff on root page is standard GO profiling tools
i.	Geared to GO profiling
ii.	Just a high-level summary
iii.	Point to GO documentation


MATTERMOST:

master DB connections - connections to master DB
read replica DB connections - connections to read replica DBs
------------------------------
http errors total - API Errors per Minute
http request duration seconds bucket -- this gives request time bucket (?)
http request duration seconds count
http request duration seconds sum -- this divided by count gives avg request time per minute
http requests total - API requests per minute
http websockets total - number of users online
login logins fail total - # of failed logins
login logins total - # of successful logins
post broadcasts total - broadcasts
post emails sent total -
post file attachments total -
post pushes sent total -
post total - messages
process cpu total
process max fds
process open fds
process resident memory bytes
process start time seconds
process virtual memory bytes

and a bunch of go routines
 - memory usage
 - go routines
 - garbace collection duration
 - objects on the heap
