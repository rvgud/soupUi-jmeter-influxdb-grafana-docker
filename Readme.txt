// Run influxdb
docker run --rm -e INFLUXDB_DB=jmeter  -e INFLUXDB_ADMIN_USER=admin -e INFLUXDB_ADMIN_PASSWORD=pass -p 8086:8086 -p 2003:2003  --net testnetwork  -e INFLUXDB_GRAPHITE_ENABLED=true   --name influxdb  influxdb

// Run grafana
docker run --rm  -d --name=grafana -p 3000:3000 --net testnetwork  grafana/grafana

//Grafana dahsboard id :- https://grafana.com/grafana/dashboards/3351
// Run SoapUi MockServer
docker run --net testnetwork -v "$(pwd)/soapui-prj:/home/soapui/soapui-prj/" -P -p 8080:8080 -it --rm -e MOCK_SERVICE_NAME=PDF -e PROJECT=/home/soapui/soapui-prj/PDFetchData-soapui-project.xml --name soapuimock fbascheper/soapui-mockservice-runner


//Run Jmeter test

docker run --net testnetwork  -v "$(pwd)/jmetertest:/mnt/jmeter" justb4/jmeter -n   -t /mnt/jmeter/pdfDataTestPlan.jmx  -l /mnt/jmeter/tmp/result_${timestamp}.jtl   -j /mnt/jmeter/tmp/jmeter_${timestamp}.log 
