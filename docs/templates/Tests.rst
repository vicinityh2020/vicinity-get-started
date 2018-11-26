There have been set up some tests to check some of the functionalities of the platform. There is a special focus on the API. We will see in this section how to run the tests.

.. note:: This functionality is available for the server only. (Navigate to the server root folder to execute the scripts that you will find below)

1. Set up a test db

(In the section `Platform deployment <https://github.com/vicinityh2020/vicinity-neighbourhood-manager/wiki/Deployment-of-the-platform>`_, there is more info regarding how to setup Mongo, create users or add authorisation to access databases)

    * Create a clean mongo database. We will assume the name "nmapitest".
    * Authorize one user to make use of this database. We will assume user and password to be "test".
    * Insert a first user in "users" and organisation in "useraccounts" into the db "nmapitest":

  ::

      db.useraccounts.insert({
          "avatar" : "",
          "location" : "SomeCountry",
          "cid" : "admin-test",
          "name" : "adminCorp",
          "businessId" : "1234-5678",
          "hasNotifications" : [],
          "knowsRequestsTo" : [],
          "knowsRequestsFrom" : [],
          "knows" : [],
          "status" : "active",
          "skinColor" : "blue"
      })

  ::

      db.users.insert({
        "email" : "admin@admin.com",
        "occupation" : "Web Master",
        "avatar" : "",
        "name" : "admin",
        "authentication" : {
            "hash" : "$2a$10$EFuFLZGxhfgScAnR/.aj9Osnbw/4sTmHcW2guFesyIChSr8w/bSHC",
            "principalRoles" : [
                "user",
                "administrator",
                "infrastructure operator",
                "service provider",
                "device owner",
                "system integrator",
                "devOps",
                "superUser"
                ]
            },
            "accessLevel" : 2.0,
            "status" : "active"
          })

* Then update them as follows. Remember to replace the id with ObjectId(" ID OF THE NEW ORGANISATION OR USER ") that you obtained in the previous step:

  ::

      db.useraccounts.update(
        {cid: "admin-test"},
        {$set: {
             "accountOf" : [ {
              "id" : < OBTAINED WHEN INSERTING >,
              "extid" : "admin@admin.com"
             }
          ]}
        })

  ::

      db.useraccounts.update(
        {email : "admin@admin.com"},
        {$set: {
             "cid" : {
                 "id" : < OBTAINED WHEN INSERTING >,
                 "extid" : "admin-test"
                 }
              }
          })

2. Once the db is ready replace in the package JSON the script test with the following:

  ::

    "test": "env=test VCNT_MNGR_DB=mongodb://test:test@ < YOUR IP or LOCALHOST > :27017/nmapitest nyc mocha './test/**/*.spec.js'"

3. Finally, to run the test execute:

  ::

    npm run test
