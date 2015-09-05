Simple LDAP User
================

This module allows authentication to the LDAP directory configured in the
Simple LDAP module. It also provides synchronization services both to and from
LDAP and Drupal. It supports mapping LDAP attributes to Drupal user object
fields (both native, and using Field API).

Configuration
=============

In addition to the configuration available in the administration UI, an
attribute map can be specified in settings.php, using the variable
$conf['simple_ldap_user_attribute_map'].

This variable is an array of key => value pairs.  The keys are the LDAP
attributes (lowercased) and the values are arrays of Drupal User field names.
This must be the machine name of the field.

If two or more Drupal User field names are present, an entry with the key
'#delimiter' MAY be added to mark how the LDAP field should be split.  If no
'#delimiter' entry is present, then Simple LDAP will map the user fields ot
a multivalue LDAP attribute.  Typical '#delimiter' values are '$' and "\r".

The administration page at admin/config/people/simple_ldap/profile will create
a valid simple_ldap_user_attribute_map for most installs.

Example:
--------
$conf['simple_ldap_user_attribute_map'] = array(

  // Generic example.
  'ldap-attribute' => array(
    'drupal-user-field-machine-name',
  ),

  // First name example.
  'givenname' => array(
    'field_first_name',
  ),

  // Last name example.
  'sn' => array(
    'field_last_name',
  ),

  // Timezone example (saved directly to users table, note there is no '#').
  'l' => array(
    'timezone',
  ),

  // Combined fields example.
  'displayname' => array(
      'field_first_name',
      'field_last_name',
      '#delimiter' => ' ',
    ),
  ),

  'street' => array(
    'field_street_1',
    'field_street_2',
    '#delimiter' => "\r",
  ),

);

Active Directory Example:
-------------------------
$conf['simple_ldap_user_attribute_map'] = array(
  'givenname' => array(
    'field_first_name',
  ),
  sn => array(
    'field_last_name',
  ),
  'cn' => array(
    'field_first_name',
    'field_last_name',
  ),
  'displayname' => array(
    'field_first_name',
    'field_last_name',
    '#delimiter' => ' ',
  ),
);

Testing
=======

The simpletests provided with this module automatically configure themeselves
to use the active configuration in order to perform a real-world test against
your real LDAP server.

THIS MEANS THAT DATA WILL BE ADDED/DELETED ON YOUR REAL LDAP SERVER!

The simpletests only operate against entries it creates, but in the event of a
failure, the test cannot clean up after itself. If you are testing a specific
configuration, it is recommended to run the test against a development or
staging directory first.
