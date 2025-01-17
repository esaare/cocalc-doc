query
=====

-  ``id``: A unique UUID for the query
-  ``query``: The actual query (required)
-  ``changes``:
-  ``multi_response``: (default: false)
-  ``options``:

This queries directly the database (sort of Facebook’s GraphQL) Options
for the ‘query’ API message must be sent as JSON object. A query is
either *get* (read from database), or *set* (write to database). A query
is *get* if any query keys are null, otherwise the query is *set*.

Note: queries with ``multi_response`` set to ``true`` are not supported.

Examples of *get* query:
^^^^^^^^^^^^^^^^^^^^^^^^

Get title and description for a project, given the project id.

::

     curl -u sk_abcdefQWERTY090900000000: -H "Content-Type: application/json" \
       -d '{"query":{"projects":{"project_id":"29163de6-b5b0-496f-b75d-24be9aa2aa1d","title":null,"description":null}}}' \
       https://cocalc.com/api/v1/query
     ==> {"event":"query",
          "id":"8ec4ac73-2595-42d2-ad47-0b9641043b46",
          "query":{"projects":{"project_id":"29163de6-b5b0-496f-b75d-24be9aa2aa1d",
                               "title":"MY NEW PROJECT 2",
                               "description":"desc 2"}},
          "multi_response":false}

Get info on all projects for the account whose security key is provided.
The information returned may be any of the api-accessible fields in the
``projects`` table. These fields are listed in CoCalc source directory
src/smc-util/db-schema, under ``schema.projects.user_query``. In this
example, project name and description are returned.

Note: to get info only on projects active in the past 3 weeks, use
``projects`` instead of ``projects_all`` in the query.

::

     curl -u sk_abcdefQWERTY090900000000: -H "Content-Type: application/json" \
       -d '{"query":{"projects_all":[{"project_id":null,"title":null,"description":null}]}}' \
       https://cocalc.com/api/v1/query
     ==> {"event":"query",
          "id":"8ec4ac73-2595-42d2-ad47-0b9641043b46",
          "multi_response": False,
          "query": {"projects_all": [{"description": "Synthetic Monitoring",
                            "project_id": "1fa1626e-ce25-4871-9b0e-19191cd03325",
                            "title": "SYNTHMON"},
                           {"description": "No Description",
                            "project_id": "639a6b2e-7499-41b5-ac1f-1701809699a7",
                            "title": "TESTPROJECT 99"}]}}

Get project id, given title and description.

::

     curl -u sk_abcdefQWERTY090900000000: -H "Content-Type: application/json" \
       -d '{"query":{"projects":{"project_id":null,"title":"MY NEW PROJECT 2","description":"desc 2"}}}' \
       https://cocalc.com/api/v1/query
     ==> {"event":"query",
          "query":{"projects":{"project_id":"29163de6-b5b0-496f-b75d-24be9aa2aa1d",
                               "title":"MY NEW PROJECT 2",
                               "description":"desc 2"}},
          "multi_response":false,
          "id":"2be22e08-f00c-4128-b112-fa8581c2d584"}

Get users, given the project id.

::

     curl -u sk_abcdefQWERTY090900000000: \
       -H "Content-Type: application/json" \
       -d '{"query":{"projects":{"project_id":"29163de6-b5b0-496f-b75d-24be9aa2aa1d","users":null}}}' \
       https://cocalc.com/api/v1/query
     ==> {"event":"query",
          "query":{"projects":{"project_id":"29163de6-b5b0-496f-b75d-24be9aa2aa1d",
                               "users":{"6c28c5f4-3235-46be-b025-166b4dcaac7e":{"group":"owner"},
                                        "111634c0-7048-41e7-b2d0-f87129fd409e":{"group":"collaborator"}}}},
          "multi_response":false,
          "id":"9dd3ef3f-002b-4893-b31f-ff51440c855f"}

Show project upgrades. Like the preceding example, this is a query to
get users. In this example, there are no collaborators, but upgrades
have been applied to the selected project. Upgrades do not show if none
are applied.

The project shows the following upgrades: - cpu cores: 1 - memory: 3000
MB - idle timeout: 24 hours (86400 seconds) - internet access: true -
cpu shares: 3 (stored in database as 768 = 3 \* 256) - disk space: 27000
MB - member hosting: true

::

     curl -u sk_abcdefQWERTY090900000000: \
       -H "Content-Type: application/json" \
       -d '{"query":{"projects":{"project_id":"29163de6-b5b0-496f-b75d-24be9aa2aa1d","users":null}}}' \
       https://cocalc.com/api/v1/query
     ==> {"event":"query",
          "query":{"projects":{"project_id":"29163de6-b5b0-496f-b75d-24be9aa2aa1d",
                               "users":{"6c28c5f4-3235-46be-b025-166b4dcaac7e":{
                                            "group":"owner",
                                            "upgrades":{"cores":1,
                                                        "memory":3000,
                                                        "mintime":86400,
                                                        "network":1,
                                                        "cpu_shares":768,
                                                        "disk_quota":27000,
                                                        "member_host":1}}}}},
          "multi_response":false,
          "id":"9dd3ef3f-002b-4893-b31f-ff51440c855f"}

Get editor settings for the present user.

::

     curl -u sk_abcdefQWERTY090900000000: \
       -H "Content-Type: application/json" \
       -d '{"query":{"accounts":{"account_id":"29163de6-b5b0-496f-b75d-24be9aa2aa1d","editor_settings":null}}}' \
       https://cocalc.com/api/v1/query
     ==> {"event":"query",
          "multi_response":false,
          "id":"9dd3ef3f-002b-4893-b31f-ff51440c855f",
          "query": {"accounts": {"account_id": "29163de6-b5b0-496f-b75d-24be9aa2aa1d",
                                 "editor_settings": {"auto_close_brackets": True,
                                                     "auto_close_xml_tags": True,
                                                     "bindings": "standard",
                                                     "code_folding": True,
                                                     "electric_chars": True,
                                                     "extra_button_bar": True,
                                                     "first_line_number": 1,
                                                     "indent_unit": 4,
                                                     "jupyter_classic": False,
                                                     "line_numbers": True,
                                                     "line_wrapping": True,
                                                     "match_brackets": True,
                                                     "match_xml_tags": True,
                                                     "multiple_cursors": True,
                                                     "show_trailing_whitespace": True,
                                                     "smart_indent": True,
                                                     "spaces_instead_of_tabs": True,
                                                     "strip_trailing_whitespace": False,
                                                     "tab_size": 4,
                                                     "theme": "default",
                                                     "track_revisions": True,
                                                     "undo_depth": 300}}}}

Examples of *set* query.
^^^^^^^^^^^^^^^^^^^^^^^^

Set title and description for a project, given the project id.

::

     curl -u sk_abcdefQWERTY090900000000: \
       -H "Content-Type: application/json" \
       -d '{"query":{"projects":{"project_id":"29163de6-b5b0-496f-b75d-24be9aa2aa1d", \
                                 "title":"REVISED TITLE", \
                                 "description":"REVISED DESC"}}}' \
       https://cocalc.com/api/v1/query
       ==> {"event":"query",
            "query":{},
            "multi_response":false,
            "id":"ad7d6b17-f5a9-4c5c-abc3-3823b1e1773f"}

Make a path public (publish a file).

::

     curl -u sk_abcdefQWERTY090900000000: \
       -H "Content-Type: application/json" \
       -d '{"query":{"public_paths":{"project_id":"29163de6-b5b0-496f-b75d-24be9aa2aa1d", \
                                     "path":"myfile.txt", \
                                     "description":"a shared text file"}}}' \
       https://cocalc.com/api/v1/query
       ==> {"event":"query",
            "query":{},
            "multi_response":false,
            "id":"ad7d6b17-f5a9-4c5c-abc3-3823b1e1773f"}

Add an upgrade to a project. In the “get” example above showing project
upgrades, change cpu upgrades from 3 to 4. The ``users`` object is
returned as read, with ``cpu_shares`` increased to 1024 = 4 \* 256. It
is not necessary to specify the entire ``upgrades`` object if you are
only setting the ``cpu_shares`` attribute because changes are merged in.

::

     curl -u sk_abcdefQWERTY090900000000: \
       -H "Content-Type: application/json" \
       -d '{"query":{"projects":{"project_id":"29163de6-b5b0-496f-b75d-24be9aa2aa1d", \
                                 "users":{"6c28c5f4-3235-46be-b025-166b4dcaac7e":{ \
                                              "upgrades": {"cpu_shares":1024}}}}}}' \
       https://cocalc.com/api/v1/query
       ==> {"event":"query",
            "query":{},
            "multi_response":false,
            "id":"ec822d6f-f9fe-443d-9845-9cd5f68bac20"}

Set present user to open Jupyter notebooks in “CoCalc Jupyter Notebook”
as opposed to “Classical Notebook”. This change not usually needed,
because accounts default to “CoCalc Jupyter Notebook”.

It is not necessary to specify the entire ``editor_settings`` object if
you are only setting the ``jupyter_classic`` attribute because changes
are merged in.

::

     curl -u sk_abcdefQWERTY090900000000: \
       -H "Content-Type: application/json" \
       -d '{"query":{"accounts":{"account_id":"29163de6-b5b0-496f-b75d-24be9aa2aa1d","editor_settings":{"jupyter_classic":false}}}}' \
       https://cocalc.com/api/v1/query
     ==> {"event":"query",
          "multi_response":false,
          "id":"9dd3ef3f-002b-4893-b31f-ff51440c855f",
          "query": {}}

**NOTE:** Information on which fields are gettable and settable in the
database tables via API message is in the directory ‘db-schema’, in
CoCalc sources on GitHub at
https://github.com/sagemathinc/cocalc/blob/master/src/smc-util/db-schema

Within directory ‘db-schema’:

-  for *project* fields you can get, see the definition of
   ``schema.projects.user_query.get.fields``
-  for *project* fields you can set, see the definition of
   ``schema.projects.user_query.set.fields``
-  for *user account* fields you can get, see the definition of
   ``schema.accounts.user_query.get.fields``
-  for *user account* fields you can set, see the definition of
   ``schema.accounts.user_query.set.fields``

