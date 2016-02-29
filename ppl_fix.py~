__author__ = 'user'

import json, csv, copy, uuid, time
import yaml
import urllib2, base64
import sys, getopt

# opensrp server url
BIDAN_BASE_URL = "http://localhost:9979/"
FORM_SUBMISSIONS_PATH = "form-submissions"

all_form = []
csv_data = []

csv_file = ''
json_file = ''

need_to_post = False

# Handle argument
try:
    opts, args = getopt.getopt(sys.argv[1:], "hc:j:s", ["ifile=","ofile="])
except getopt.GetoptError:
    print 'test.py -i <inputfile> -o <outputfile>'
    sys.exit(2)
for opt, arg in opts:
    if opt == '-h':
        print 'test.py -c <csv_file> -j <json_file>'
        sys.exit()
    elif opt in ("-c", "--ifile"):
        csv_file = arg
    elif opt in ("-j", "--ofile"):
        json_file = arg
    elif opt == '-s':
        need_to_post = True

if not csv_file or not json_file:
    print 'test.py -c <csv_file> -j <json_file>'
    sys.exit()

# Open JSON file
try:
    print "Try to open json file..."
    with open(json_file+".json") as data_file:
        data = yaml.safe_load(data_file)
    print "Successfully opened json file"
except IOError as e:
    print e
    sys.exit()

# Open CSV file
try:
    print "Try to open csv file..."
    with open(csv_file+".csv", 'r') as csv_in:
        reader = csv.DictReader(csv_in)
        for row in reader:
            csv_data.append(row)
    print "Successfully opened csv file"
except IOError as e:
    print e
    sys.exit()

for idx, val in enumerate(csv_data):
    json_data = copy.deepcopy(data)
    form = dict()
    for idx2, val2 in enumerate(data['form']['fields']):
        # Add source value
        source = json_data['form']['fields'][idx2].get('source');
        if not source:
            json_data['form']['fields'][idx2]['source'] = json_data['form']['bind_type'] + "." + val2['name']
        if val2['name'] in val.keys() and val[val2['name']] != "-":
            json_data['form']['fields'][idx2]['value'] = val[val2['name']]

    form['anmId'] = val['userId']
    form['entityId'] = val['id']
    form['clientVersion'] = str(int(round(time.time() * 1000)))
    form['formDataDefinitionVersion'] = json_data['form_data_definition_version']
    form['formInstance'] = json.dumps(json_data)
    form['formName'] = csv_file
    form['instanceId'] = str(uuid.uuid4())
    all_form.append(form)

if need_to_post:
    # Post forms
    try:
        req = urllib2.Request(BIDAN_BASE_URL + FORM_SUBMISSIONS_PATH)
        req.add_header('Content-Type', 'application/json')
	# change with one of your opensrp client username and password 
        base64String = base64.encodestring('%s:%s' % ("your_user_here", "your_password_here")).replace('\n', '')
        req.add_header('Authorization', "Basic %s" % base64String)
        response = urllib2.urlopen(req, json.dumps(all_form))
        print "Posted"
    except(urllib2.HTTPError, urllib2.URLError) as e:
        print(e)
else:
    print(all_form)

