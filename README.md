graphite-instalation-on-debian
==============================
sudo mkdir /opt/graphite
sudo apt-get install python-virtualenv python-cairo libcairo-dev python-dev
sudo chown user:user /opt/graphite
virtualenv /opt/graphite
cd /opt/graphite
rm lib/python2.7/no-global-site-packages.txt
source bin/activate 
pip -v install graphite-web carbon whisper django django-tagging uwsgi 
cp /opt/graphite/conf/carbon.conf.example /opt/graphite/conf/carbon.conf
cp /opt/graphite/conf/storage-schemas.conf.example /opt/graphite/conf/storage-schemas.conf
cd webapp/graphite
mv local_settings.py.example local_settings.py
vim local_settings.py
python manage.py syncdb
cd ../../
bin/carbon-cache.py start
uwsgi --http localhost:8085 --master --processes 4 --home /opt/graphite --pythonpath /opt/graphite/webapp/graphite --wsgi-file=/opt/graphite/conf/graphite.wsgi.example

