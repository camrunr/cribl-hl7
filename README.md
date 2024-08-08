# Cribl HL7
----

The Cribl HL7 Pack will parse the top level segments present in an HL7 v2.5 log event. The result is JSON fields for each segment, but sub-fields are not further parsed in current form. Field names are based on normalized versions of those found in HL7 schema. The original _raw event will be dropped by default, but this behavior is easily changed. We do not make use of the MSH lead delimiting character, nor the `encoding_characters` field. Instead we assume `|` and `^` delimiters in the data.

Example of a simple event:
```
MSH|^~\&|SystemA|HosA|PI|MDM|2008090801529||ADT^A01|000000_CTLID_2008090801529|P|2.3.1|||AL|NE
EVN|A01|2008090801529|||JavaCAPS6^^^^^^^USERS
```

Would result in this structure:
```
'MSH': {
  'accept_acknowledgment_type': 'AL',
  'application_acknowledgment_type': 'NE',
  'date_time_of_message': '20080908013332',
  'encoding_characters': '^~\&',
  'message_control_id': '000001_CTLID_20080908013332',
  'message_type': 'ADT^A01',
  'processing_id': 'P',
  'receiving_application: 'PI',
  'receiving_facility': 'MDM',
  'sending_application': 'SystemA',
  'sending_facility': 'HosA',
  'version_id': '2.3.1'
},
'EVN': {
  'event_type_code': 'A01',
  'operator_id': 'JavaCAPS6^^^^^^^USERS',
  'recorded_date_time': '2008090801529'
}
```

The subfields `operator_id` and `message_type` are not parsed. (see note below re: testing)

All field names can be found in the Pack's Knowledge -> Global Variables section. Click the Custom option to see just the variables used by this Pack.

## Requirements and Use

**Read this entire section**

The Pack assumes HL7 version 2.5. Earlier versions were not tested, checked, looked at, or sampled.

If you do not plan on further working on the events, simply create a route and point it at the Pack.

If you'd like to perform more work on the events, I recommend using the Chain function to call the Pack. Example, create a Pipeline with a Chain function as rule 1. The Chain points to the Pack. Once the Pack has worked on the data, and created the JSON structure, the event will return to your Pipeline where you can further enhance it. Add desired functions after the Chain. Finally, set-up a Route that points to this new Pipeline.

The Cribl admin will need to decide what to do with the parsed data. By default the extracted segments are merely kept as top level fields. Some destinations will deal with this propperly. Others may plunk it into the waste bin. There is a disabled example of another option: Serialize it back into _raw as stringified JSON. There is also an Eval function to drop the original _raw data by default. Disable if this is not desired.

**TESTING** In the Code function there is an option to parse subfields into arrays. Use with caution, and please provide feedback if you do try it.


## Release Notes

### Version 1.0.1 - 2024-08-08
* Updated the description
* Added github info in this doc
* NO FUNCTIONAL CHANGES MADE TO THE PACK

### Version 1.0.0 - 2024-07-31
* Allowed for \n as well as \r in segment separators
* Added debugging flag in Code function (off by default)
* Added subfield parsing option in Code function (off by default)
* Created this Fine Readme

### Version 0.1.2 - 2024-07-30
* Fixed MSH segment type offset by 1

### Version 0.1.1 - 2024-07-30
* Added all 176 segment codes
* Fixed the `segments` clean-up code

### Version 0.0.2 - 2024-07-29
* 1st pre-release

## Contributing to the Pack
The Pack source can be found in [GitHub](https://github.com/camrunr/cribl-hl7). Feel free to fork the repo and have your way. Issue a PR if you think your changes are useful to others, and we'll review.

## Contact
Jon Rust <jrust@cribl.io>


## License
This Pack uses the Apache 2.0 license

