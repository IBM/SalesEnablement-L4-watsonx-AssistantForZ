mkdir myDocs

oc -n wxa4z-byos get secret wxa4z-client-ingestion-tls-secret -o jsonpath="{.data.ca\.crt}" | base64 -d > ca.crt

ls -l ca.crt

export CA_PATH=ca.crt

mkdir index

mkdir index/redpieces

cp /Volumes/RedHotDrive/2024-Projects/watsonxAssistant4Z/Level\ 4/redp5340-compressed.pdf index/redpieces

ls -l index/redpieces

zassist init index

zassist status index

echo https://$(oc -n wxa4z-byos get route wxa4z-client-ingestion -o jsonpath="{.spec.host}")

oc -n wxa4z-byos get secret client-ingestion-authkey -o jsonpath="{.data.authkey}" | base64 -d

zassist login https://wxa4z-client-ingestion-wxa4z-byos.apps.672b79320c7a71b728e523b4.ocp.techzone.ibm.com

zassist ingest index

zassist load index

zassist status index
