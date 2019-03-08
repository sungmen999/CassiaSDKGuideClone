Cassia provides two sets of RESTful APIs that enable BLE device interaction with Cassia
routers:
  * APIs on the local router (where the application is usually on the same network as the
router)
  * APIs through Cassia’s IoT Access Controller (Cassia AC).
The APIs below are only available through the Cassia AC. Except for these APIs, the two set
of RESTful APIs are the same and will give the same result. In this document, we use the
APIs through Cassia AC as examples.
  * Positioning APIs (chapter 5.4)
  * Obtain Cassia router’s status (chapter 5.2.2)
  * Monitor Cassia router’s status APIs (chapter 5.2.3)
  * Obtain all online routers’ status (chapter 5.2.4)
  * Router auto-selection (chapter 5.6, introduced in firmware 1.3)
  * SSE Combination (chapter 5.7, introduced in firmware 1.3)

NOTE:
  * The RESTful APIs through Cassia AC includes “/api” after {your AC domain}. It is not
needed for the RESTful APIs on the local router.
  * The RESTful APIs through Cassia AC includes “mac=<mac>” to identify which router is
used. It is not needed for the RESTful APIs on the local router.
  * For firmware 1.2 or below, if you want to use RESTful APIs on the local routers, you
need to turn on Local RESTful API in AC console or router console. Please see below
screenshots.

Figure 1: (v1.2) Turn on Local RESTful API in AC Console

Figure 2: (v1.2) Turn on Local RESTful API in Router Console

In firmware 1.3, local RESTful API will be automatically turned on, if the router is
configured as Standalone Mode. If the router is configured as AC Managed, the local
RESTful API will be turned off. Please see below screenshots.

Figure 3: (v1.3) Configuration of Router Mode on Router Console
