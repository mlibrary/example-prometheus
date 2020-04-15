# Example of Rails, Sidekiq and Puma metrics exported to Prometheus + Grafana

This example illustrates how to use [yabeda] gem suite with rails application to draw graphs for app metrics.

## Prerequisites

Recent versions of Docker and Docker Compose installed.

## Usage

- Execute `docker-compose up` to start.
- Go to rails application at http://localhost:5000
- Hit the button and refresh page few times
- Go to Grafana Web UI at [localhost](http://localhost:3000/d/000000001/yabeda-metrics-for-rails-sidekiq-and-puma?refresh=5s) (user: `admin`/`admin`)
- Look for graphs
- After starting up docker-compose will run `rails-stressor` service to simulate huge load for the rails app. By default will do the request for 2 minutes.
Settings could be changed in `docker-compose.yml` config (command parameter of the `rails_stressor` service).

You also could run rails stressor again by executing `docker-compose up rails_stressor` command.

## Notes

- Sample [Rails] application is equipped with [yabeda-rails], [yabeda-sidekiq], [yabeda-puma-plugin], and [yabeda-prometheus] gems and properly configured.
- Raw rails metrics are exposed at http://localhost:5000/metrics
- Raw sidekiq metrics are exposed at http://localhost:5100/metrics
- Raw puma metrics are exposed at http://localhost:5100/metrics
- The [Prometheus] Web UI runs at http://localhost:9090
- The [Grafana] Web UI runs at http://localhost:3000 , user: `admin`/`admin`.
- The [Sidekiq] Web UI is available at http://localhost:5000/sidekiq
- Now includes Solr admin: http://localhost:8983/solr/#/
- Solr-Exporter: http://localhost:9854/metrics

## Possible errors and their solutions

### Permission denied error / My grafana container is not running (Stopping after boot)

If your grafana's container is not running correctly after you run `docker-compose up` you're probably getting this error, if you check at `docker ps -a` you'll see your grana container with the status `Exited`, picks the id of this container and run `docker logs YOUR_GRAFANA_CONTAINER_ID` and check if the errors is equal to the following:

```
GF_PATHS_DATA='/var/lib/grafana' is not writable.
You may have issues with file permissions, more information here: http://docs.grafana.org/installation/docker/#migration-from-a-previou
s-version-of-the-docker-container-to-5-1-or-later
mkdir: cannot create directory '/var/lib/grafana/plugins': Permission denied
```

#### Solution

Replace at your `docker-compose.yml` line 38 by your user's `id` that you will get on the following command:

```sh
id -u
```

## Acknowledgement

The configurations are based off the following articles and repositories:
 - https://finestructure.co/blog/2016/5/16/monitoring-with-prometheus-grafana-docker-part-1
 - https://github.com/NikolajLeischner/local-prometheus-grafana
 - https://www.digitalocean.com/community/tutorials/how-to-add-a-prometheus-dashboard-to-grafana
 - https://github.com/aborroy/alfresco-solr-monitoring
 - https://lucene.apache.org/solr/guide/7_3/monitoring-solr-with-prometheus-and-grafana.html

## License

This example is available as open source under the terms of the [MIT License](https://opensource.org/licenses/MIT).

[yabeda]: https://github.com/yabeda-rb/yabeda
[yabeda-rails]: https://github.com/yabeda-rb/yabeda-rails
[yabeda-sidekiq]: https://github.com/yabeda-rb/yabeda-sidekiq
[yabeda-puma-plugin]: https://github.com/yabeda-rb/yabeda-puma-plugin
[yabeda-prometheus]: https://github.com/yabeda-rb/yabeda-prometheus
[Rails]: https://rubyonrails.org "Ruby on Rails MVC web-application framework optimized for programmer happiness"
[Sidekiq]: https://github.com/mperham/sidekiq/ "Simple, efficient background processing for Ruby"
[Prometheus]: https://prometheus.io/ "Open-source monitoring solution"
[Grafana]: https://grafana.com/
[Solr and Prometheus]: https://lucene.apache.org/solr/guide/7_3/monitoring-solr-with-prometheus-and-grafana.html
