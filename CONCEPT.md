# ILIAS-LTI-Platform - A Plugin to include LTI 1.3 content in ILIAS

The plugin provides a modern and maintainable implementation to turn [ILIAS](https://www.ilias.de)
into a [LTI platform](https://www.imsglobal.org/spec/lti/v1p3/#platforms-and-tools).
The implementation is provided by [Databay AG](https://www.databay.de) and
[CaT Concepts and Training GmbH](https://www.concepts-and-training.de). This is 
created as an effort to guarantee that ILIAS can provide a high quality LTI-platform
implementation for a long time.

## Background and Goals

[LTI](https://www.1edtech.org/standards/lti) is a standard to integrate learning `tools`
(like e.g. a quizz card application) into `platforms` (such as ILIAS). The recommended
version of the standard is [LTI 1.3](https://www.imsglobal.org/spec/lti/v1p3).
It is based [OpenId Connect](https://openid.net/developers/how-connect-works/) and
[OAuth 2](https://oauth.net/2/). The core standard defines terminology and basic
mechanisms to deploy and launch tools from platforms while providing contextual
and user information. The core standard is then extended by various mandatory and
optional services that provide additional capabilities for implementing parties.

[ILIAS](https://www.ilias.de) provides an [implementation](https://docu.ilias.de/go/wiki/wpage_4335_1357)
of said standard (and the previous version LTI 1.1), as a `platform` but for some
components of ILIAS also as a `tool`. The implementation has experienced various
problems since its inception, such as problems with the underlying libraries,
challenging QA processes and issues to source ressources and funding. Nevertheless,
a functional LTI implementation is a must have for a modern learning management
system such as ILIAS.

Both participating organisations, [Databay AG](https://www.databay.de) and
[CaT Concepts and Training GmbH](https://www.concepts-and-training.de), do have
customers that want to use LTI or are already using it. If LTI is already used this
does not exclusivly happen via the ILIAS core implementation of LTI. Some customers
use the [External Content Plugin](https://github.com/DatabayAG/ExternalContent)
which also implemented LTI for ILIAS for a long time already. The use of alternative
implementations and the reluctance to fully adopt LTI content in ILIAS demonstrate
the requirement for a reliable LTI implementation for ILIAS.

Starting at that requirement, [Databay AG](https://www.databay.de) and
[CaT Concepts and Training GmbH](https://www.concepts-and-training.de) decided to
join forces in building such an implementation to fit their requirements and the 
requirements of their customers. The possibility to ask for [authorities](https://github.com/ILIAS-eLearning/ILIAS/blob/trunk/docs/development/maintenance.md#authorities)
for the existing implementation in ILIAS was well considered but discarded in the end.
There already are various expectations about, active usages of and even relations around
the existing core implementation that don't need to be disturbed. The goal, instead,
is to build an opionated LTI implementation for ILIAS from scratch with a fresh mind.

The **goals** for this implementation are:

* Provide an implementation for an LTI 1.3 platform that closely sticks to LTI 1.3
  mechanisms, possibilities and wording, in the backend and the frontend.
* Match the implications and opportunities of the standard to the facilities of ILIAS
  to integrate LTI 1.3 with ILIAS as closely as possible, as described in the 
  [strategy of the Technical Board](https://github.com/ILIAS-eLearning/ILIAS/blob/trunk/docs/development/tb-strategy.md#platform-for-learning).
* Use standard mechanisms and facilities of ILIAS, such as standard endpoints,
  ilObject, RBAC, the UI framework, as much as possible. Use modern patterns and
  approaches of the ILIAS community, such as UI framework, repository pattern and
  dependency injection as much as possible. Ensure the source code is maintainable by
  adhering to clean code principles, comprehensive documentation, and thorough testing.
  This includes using consistent coding standards, modular design, and automated testing
  to facilitate easy updates and collaboration. Overall, this should be an implementation
  that looks and feels like ILIAS, in the frontend and the backend, as much as possible.
* The implementation should provide a basis to create specialized implementations
  for specific LTI 1.3 tools, e.g. by copying the plugin repository.

The **non-goals** for this implementation are:

* Do not provide compatibility for LTI < 1.3. Using LTI 1.1 is [discouraged](https://www.imsglobal.org/specs/ltiv1p1p1/implementation-guide)
  even by its own creators.
* Compatibility to existing LTI implementations in the ILIAS ecosystem is not intended.
* Reusing code from existing LTI implementation in the ILIAS ecosystem is not intended.
* The implementation uses an existing LTI 1.3 PHP implementation to implement LTI 1.3,
  it is not intended to provide a generic LTI 1.3 implementation for PHP.
* The implementation shall not implement any vendor specific logic to accomodate certain
  LTI 1.3 tools.

## First Iteration: Basic Interface as Repository Object

As of January '25, this plugin specification has reached a maturity that warrants a
first iteration to implement the plugin. The first iteration won't support all targeted
use-cases or ideas. Instead, it shall implement the LTI interface as a basic
Repository Object, as described below. Other facilities, such as Page Editor Components,
will be added later. The first iteration shall create a usable implementation that is
up to the goals defined above. The use cases and moving parts that are subject to
the first iteration are marked with (I1).

For the first iteration, the Deep Linking Specification won't be considered.
  
## Use-Cases

This section describes the use cases for the plugin as far as they are specific to
the intended LTI platform implementation. Use cases that concern generic ILIAS
functionality, such as granting permission via RBAC or adding options in the repository
of ILIAS are listed here only if there shall be notable deviations from standard
behaviour.

The Use-Cases are based on the following specifications:

* [Core Specification](https://www.imsglobal.org/spec/lti/v1p3)
* [Assignments and Grade Service](https://www.imsglobal.org/spec/lti-ags/v2p0/)
* [Name and Role Provisioning Service](https://www.imsglobal.org/spec/lti-nrps/v2p0)
* [Deep Linking Specification](https://www.imsglobal.org/spec/lti-dl/v2p0)

and include derived use cases here.

### Deployment 

* As an **administrator** I want to include a LTI tool via the single tenant deployment
  model into my installation. (I1)
  * As a **content creator** I want to include LTI tools deployed in the single tenant
    model into my content. (I1)
* As an **administrator** I want to include a LTI tool via the multi tenant deployment
  model into my installation.
  * As a **content creator** I want to include LTI tools deployed in the multi tenant
    model into my content.
* As a **content creator** I want to include a LTI tool via the `deployment id as
  account identifier` model by attaching a certain deployment of a tool to my own
  account.
* As a **content creator** I want to include a LTI tool deployed via any model.
* As a **content creator** I want to allow a LTI tool to post scores via the AGS
  to a singular line item.

### Presentation

* As a **content creator** I want to include a LTI tool as a repository object. (I1)
* As a **content creator** I want to include a LTI tool as part of an ILIAS page
  editor page.
* As a **content creator** I want to present/embed an LTI tool directly in ILIAS
  so it appears as normal ILIAS content.
* As a **content creator** I want to be able to forward people to an external page
  that contains the LTI tool.
* As a **content creator** I want to decide which form of presentation (via page
  editor, as "normal" ILIAS content, via external page) is used for a certain LTI
  tool deplyoment.
* As a **learner** I want to have a visual indication of the tool provider and the
  mode of presentation for a certain LTI tool repository object. (I1)
* As a **learner** I want to be able to view my latest score for a certain tool,
  including the latest comment that was given. (I1)
* As an **administrator** I want to be able to attach a certain icon to a tool,
  so that this icon is used to indicate objects (etc.) that use this tool.

### Usage

_This section may contain more use cases regarding the handling of the LTI objects
by learners._

### Include Tools in the content.

_This section may contain more use cases regarding the inclusion of the LTI objects
via the ILIAS Page Editor._

### Evaluation (I1)

* As a **content creator** I want to make the ILIAS learning progress depend on
  the score for a single line item.
* As a **content creator** I want the object to automatically determine the learning
  progress based on a threshold for score that I have set.
* As an **administrator** I want to know if a tool has not posted a score so far,
  because this might indicate that a tool does not use the AGS in general which
  might lead to unexpected behaviour.

### Monitoring (I1)

* As a **tutor** I want to know, which of my learners have already used a tool.
* As a **tutor** I want to have access to the grade book of a tool to have insights
  about the performance of my learners, if the tool supports the
  "Assignment and Grade Services Specification".
* As a **tutor** I want to know the learning progress of my learners for a tool.
* As an **administrator** I want to analyze problems of learners with a certain tool.

## Facilities and Views

This section describes the individual parts that make the plugin functionality.
It lists all things that users will find in ILIAS and briefly describes their content
and functionality.

### Plugin: LTI-Object (I1)

The functionality is delivered via a ILIAS repository object plugin. The plugin
provides the core repository object and allows to add and configure tools. It also
contains the code required for the auxiliary page component plugin.

### Tool Configuration (I1)

Via the global screen service, the plugin provides an item for the main menu that
gives access to a configuration screen for tools provided in the single and multi
tenant deployment modes.

### Repository Object

#### Creation Screen (I1)

#### Content Tab (I1)

Depending on the selected "Launch Option" in the [settings tab](#settings-tab),
the tool is presented in the "Content" tab in one of the following ways:

* same window
  * a button appears in the toolbar for launching the tool.
    when clicked, the content replaces the ILIAS screen in the current window.
    The user returns to the ILIAS interface when leaving the content,
    either via navigation options provided by the tool or through browser controls.
* new window
  * a button appears in the toolbar for launching the tool in a new window. The new
    window opens the content, providing a dedicated space for interaction. Upon leaving
    the content, the window is closed automatically.
* embedded
  * the tool is displayed within an HTML iframe inside the "Content" tab. The iframe
    dynamically adjusts to fit the tool's dimensions if supported.

#### Info Tab (I1)

The "Info" tab functionality depends on the corresponding configuration in
the [settings  tab](#settings-tab).

* When enabled:
  The repository object delegates rendering to the "Info Screen" component,
  which provides the default view for the "Info" tab.
* When disabled:
  * The "Info" tab must not be displayed in the interface.
  * Direct access to the "Info" tab via its URL must be explicitly prevented.

The configred status of the "Info Screen" setting must be defined in the "List GUI"
of the repository object (see: `\ilObjectListGUI::getInfoScreenStatus`).

#### Settings Tab

The settings screen offers the following standard ILIAS components:

* properties of ilObject (title, description)
* availability (on-/offline, period)
* visibility of "Info" tab
* launch options:
  * same window
    * the content is opened in the same window and replaces the ILIAS screen.
  * new window
    * the content is opened in a new window.
  * embedded
    * the tool is opened within the ILIAS context as an embedded content.
* score for completion: set a threshold as percentage of score that a user needs
  to reach at the tool to gain the learning progress "completed".

TODO @mjansenDatabay: Add launch options

#### Participants Tab (I1)

The participants tabs shows a UI data table containing all users that have actively
interacted with the tool deployed via the repository object. The table contains
the following columns:

* username of the user that has interacted with the tool
* additional profile data provided via the according ILIAS logic ("Visible in
  Course" for the lack of possibility to add own scopes)
* date of first and last interaction
* ILIAS learning progress

It supports these actions (m = multi, s = single):

* send mail (m/s)
* view logs (m/s)
* view grades (m/s)
* delete outcome (m/s) 

#### Grade Book, Results and Scores (I1)

If there isn't any data available regardind the  “Line Item” is available, the
user is shown a corresponding message in a "MessageBox > Info" stating that
the “Grade Book” will be available once data about the “Line Item” is available.

The "Grade Book, Results and Scores" tab provides a view to the performance of the
users that have interacted with the tool. It is visible/accessible, if the tool
supports the "Assignment and Grade Services Specification" and the "write" permission
is granted for the acting user. This also applies to all subordinate views.

Two sub-tabs are presented:

1. Gradebook
2. Score History

While the "Assignment and Grade Services Specification" describes
multiple ways to manage create and manage line item, the repository object
supports the "Declarative Approach" only by creating the line item in the moment
of ressource link creation.
This approach is backwards compatible with older LTI versions.

##### Gradebook (I1)

The "Gradebook" view presents a tabular gradebook/matrix of users (rows) and
their results (the current/last submitted score) ILIAS stored for
the tool's "Line Item".

| **Username**         | **Line Item Result**  |
|-----------------------|----------------------|
| `john.doe`           | `85`                  |
| `jane.smith`         | `90`                  |
| `student123`         | `78`                  |
| `emma.brown`         | `92`                  |
| `test_user456`       | `88`                  |

The username presentation adheres to the corresponding ILIAS guidelines.

A button "Re-calculate Results" is displayed above the gradebook to re-calculate
the results of the users based on the scores provided by the context.

Below this button, a filter is provided to filter (type: text input / default value: empty)
the gradebook by its users. The entered search string should be searched in
the user's username, email and firstname/lastname.

The ordering of the gradebook is changeable for the following fields:

* Username (ascending/descending)
* Line Item Result (ascending/descending)
* additional profile data provided via the according ILIAS logic ("Visible in
  Course" for the lack of possibility to add own scopes)

Validation Scenarios:
* Empty gradebook: If no records ar given, the view should display a
  "No records" message.

Edge Cases:
* High Volume Data: Confirm the view performs efficiently when listing a
  high number of records.

##### Score History (I1)

The view presents a table of the following user score attributes for the
"Line Item" of the specific tool in the "Context" of an ILIAS
repository object:

* Username Presentation (as provided by the ILIAS user component and
it's interfaces / sortable)
* *timestamp* (formated acc. to the users' personal ILIAS
date/time presentation settings / sortable)
* *activityProgress* (sortable)
* *gradingProgress* (sortable)
* *scoreGiven* (optional, sortable)
* *scoreMaximum* (optional, sortable)
* *comment* (optional, sortable)
* *startedAt* (optional, formated acc. to the users' personal
ILIAS date/time presentation settings, sortable)
* *submittedAt* (optional, formated acc. to the users' personal
ILIAS date/time presentation settings, sortable)

Since a tool may submit multiple scores for a user for the "Line Item", the
view provides the history of score submissions. Foreach user, who interacted
with the tool, multiple rows are shown, one for each score submission.

The following filters are provided:

* Username (type: text input / default value: empty)
* Date Range (type: duration input / default values: start = \[NOW - 1 week\], end = \[NOW\])
* Activity Progress (type: select input / default value: no option selected)
  * Initialized
  * Started
  * InProgress
  * Submitted
  * Completed
* Grading Progress (type: select input / default value: no option selected),
with the following options:
  * FullyGraded
  * Pending
  * PendingManual
  * Failed
  * NotReady

Validation Scenarios:
* Empty history: If no records ar given, the view should display a
  "No records" message.

Edge Cases:
* High Volume Data: Confirm the view performs efficiently when listing a
  high number of records.
* Localized Formatting: Test with users in different timezones
  (e.g., "Europe/Berlin", "GMT") to confirm that datetime formatting adapts accordingly.

#### Learning Progress Tab (I1)

If there isn't any data available regardind the  “Line Item” is available, the
user is shown a corresponding message in a "MessageBox > Info" stating that
the “Grade Book” will be available once data about the “Line Item” is available.

The learning progress tab provides screens to optionally enable and access the
learning progress of ILIAS users of the tool. Furthermore, the regular access
checks apply for all subordinated views with this tab.

The learning progress is disabled by default and can be enabled in the "Settings"
(sub-)tab. The settings screen must provide the following modes:

1. Learning Progress is Deactivated (default)
2. Use Score from Tool to determine Learning Progress according to Settings

If the learning progress is enabled, a button "Re-calculate Learning Progress" is
displayed on top of the settings form. A click on the button re-calculates the
learning progress for all users based on the given results.

Furthermore, the following sub-tabs are available if the learning progress is
enabled:

1. Users
2. Summary

The views should be based on core components in terms of the elements on the
user interface and the data presented. 

##### Users

The "Users" view presents a list of users who have actively interacted with the
repository object (tool). Alongside with default user profile data the list must
contain the following information:

* First Access
* Last Access
* Access Number
* Time Spent
* Percentage
* Status
* Last Status Change
* Mark
* Remark

The "Mark" and "Remark" properties must be editable, similar to other object types.

All data is formatted according to existing ILIAS guidelines and the generic
implementations in the "Tracking" component.

##### Summary

The "Summary" view must present aggregated/accumulated statistics
about the learning progress of all users that have interacted with the tool.
Similar to other learning progress enabled objects in ILIAS and in addition to
the object title and aggregated numbers of supported user profile fields, the view
provides information about:

* ∑ Users
* ∑ Access Number
* Ø Access Number / User
* Ø Time Spent / User
* Ø Time Spent / Access
* Ø Percentage / User
* Status (grouped by the different learning progress status types)
* Last Status Change
* Mark
* First Access
* Last Access

#### Protocol (I1)

The protocol view presents a detailed and transparent overview of the incoming/outgoing
communication stream. It is visible/readable if the "write" permission is granted
for the acting user.

The view is designed to help the the viewer quickly identify and
understand the sequence of events and API calls, and their respective payloads.
This transparency significantly reduces efforts in troubleshooting and bug
hunting during productive use of the platform, ensuring smooth integration
and reliable operation.

A filter bar with a date range filter should be incorporated to enhance usability.
This feature will allow users to narrow down the protocol data to specific periods,
facilitating more efficient troubleshooting and analysis.

Validation Scenarios:
* Empty Table: If no events are recorded, the table should display a
  "No records" message.
* Localized Formatting: Test with users in different timezones
  (e.g., "Europe/Berlin", "GMT") to confirm that datetime formatting adapts accordingly.
* Data Completeness: Ensure all relevant columns are populated.

Edge Cases:
* Large Payloads: Verify the table handles large serialized payloads
  (e.g., truncation or expandable sections).
* High Volume Data: Confirm the view performs efficiently when listing a
  high number of events.

Expected Data Presentation Format:

| **Date & Time**       | **URL**                 | User | **Request Header**                                      | **Request Body**                                                                                                         |
|-----------------------|-------------------------|-------------------|---------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------|
| `2024-11-22 15:35:45` | URL/scores              | zeus              | Content-Type: application/vnd.ims.lis.v1.score+json ... | `{ "scoreGiven": 85, "userId": "123" }`                                                                                  |
| `2024-11-22 15:32:12` | URL/deep-linking-launch | odysseus          | Content-Type: application/x-www-form-urlencoded ...     | `{ "iss": "962fa4d8-bcbf-49a0-94b2-2de05ad274af", "https://purl.imsglobal.org/spec/lti-dl/claim/content_items": [...] }` |

Because some rquests contain data wrapped in a "JSON Web Token", the log
should always show the decoded shape of a "JSON Web Token".

The following filters are provided:

* Date Range (type: duration input / default values: start = \[NOW - 1 week\], end = \[NOW\])
* User (type: text input / default value: empty)
  * The entered search string should be searched in the user's username, email and firstname/lastname.

(Vielleicht hier noch den "angemeldeten User" aufnehmen? - RK)

#### Metadata (Discovery/Searchability) (I1)

The repository object will not support the metadata component integration.
In the context of a tool consumer there is no need to manage metadata.
Metadata in ILIAS is used for searchability and for publishing objects
located in the "Public Area" into "Open Graph"-enabled platforms.

As for other objects the title and description of the LTI tool repository objects
are indexed for the purpose of finding them via the ILIAS search.

If we decide to support them later on, the "Metadata" component could be easily
integrated.

#### Export/Import (I1)

Export/Import of the repository object is not supported.

As user and learning data are not exported for data protection reasons in accordance
with the ILIAS guidelines, we do not see any added value in simply exporting/importing
settings and/or optional line item data for a tool.

#### RBAC and Permissions Tab (I1)

The permissions screen shows the typical matrix of roles and permissions. The plugin
therefore only consumes the corresponding interfaces provided by the "RBAC" component.

The repository object type must support and respect the following
default "RBAC" operations:

* Visible
* Read
* Copy
* Edit Settings
* Delete
* Change Permissions
* View learning progress of other users
* Edit Learning Progress Settings

### Page Component Plugin

The core functionality can be enhanced via a page component plugins. The plugin
only hooks the code from the main plugin with the page editor system of ILIAS.
Once the component revision is implemented and one plugin can hook into multiple
ILIAS systems, the page component plugin can go away and the main plugin can
provide the desired functionality directly.
As long as the component revision is not fully implementated, the communication
between both plugins should be done via well defined interfaces.
The page component plugin must not depend on a concrete implementation or any
kind of "duck typing" interface of the repository object plugin.

## Data Model

This section describes the data that the plugin needs to manage itself. It does
not describe hooks into data of other ILIAS systems that the plugin uses via interfaces.

### Tool Deployment

### Tool Deployment Communication Log

#### **Table:** Communication Log (`lti_log`) (I1)

* **lti_obj_id: int**: foreign key in `object_data`
* **user_id: int(nullable)**: foreign key in `user_data`
* **url: text**: the complete URL that was used for the interaction
* **timestamp: datetime**: the moment the communication happened
* **outgoing: bool**: is the communication outgoing or incoming
* **header: clob**: the header of HTTP request
* **payload: blob**: the content of HTTP request
 

### Assignment and Grading Service (I1)

This models the data objects that the [AGS](https://www.imsglobal.org/spec/lti-ags/v2p0/)
of LTI require. General idea is to model the data from the service faithfully. This
will make it possible to record all data that is send by an LTI tool and only interpret
it later. The other option would be to implement a custom data model and discard or
reinterpret data on receival. This would mean, though, that the interpretation can not
be changed later on.

#### **Table:** Line Item (`lti_line_item`)

* **lti_obj_id: int**: foreign key in `object_data`
* **item_id: int**: starts at 1 for each lti object, primary together with `lti_obj_id`
* **label: text**
* **resourceId: text(nullable)**
* **tag: text(nullable)**
* **scoreMaximum: float**
* **start: datetime(nullable)**
* **end: datetime(nullable)**
* **gradesReleased: bool(nullable)**
 
**resourceLinkId** is not included in the table, as since the documentation of AGS
suggest that this property is only to be used if a tool creates line items on its
own. Currently the plugin is not looking to support this feature.

#### **Table:** Scores (`lti_score`)

* **lti_obj_id: int**: foreign key in `object_data`
* **item_id: int**: foreign key in `lti_line_item` together with `lti_obj_id`
* **score_id: int**: starts at 1 for each (`lti_obj_id`, `item_id`), primary key together with (`lti_obj_id`, `item_id`)
* **user_id: int**: foreign key in `user_data`
* **scoring_user_id(nullable): int**: foreign key in `user_data`
* **score_given: float(nullable)**
* **score_maximum: float(nullable)**
* **activity_progress: string**: one of "Initialized", "Started", "InProgress", "Submitted", "Completed"
* **grading_progress: string**: one of "FullyGraded", "Pending", "PendingManual", "Failed", "NotReady"
* **timestamp: datetime**: must support sub second precision
* **started_at: datetime(nullable)**: must support sub second precision
* **submitted_at: datetime(nullable)**: must support sub second precision
* **comment: string(nullable)**

## Implementation

### Repository for Assessments and Grading (I1)

The current state of one user at a certain LTI object is one discrete domain of the
LTI plugin and hence covered by one repository and according data objects. The repo
shall provide methods to:

* cover the requirements of the "Gradebook" view
* cover the requirements of the "Score History" view
* derive a value for the current learning progress of users according to the
  data from the AGS


#### Mapping of ActivityProgress and GradingProgress from AGS to ILIAS' Learning Progress (I1)

* If the **GradingProgress** is **FullyGraded**, the setting for the scoring
  threshold is used to either set the learning progress to **Completed** or
  **Failed**.
* If the **GradingProgress** is **not FullyGraded** and the **ActivityProgress**
  is **Initialized**, the learning progress is **Not Attempted**.
* Otherwise, the learning progress is **In Progress**.

### Repository for Log (I1)

The storage and review of communication logs with certain tools is a discrete domain
to be found in the LTI repository plugin. That domain shall be covered with an according
repository, which shall provide methods to:

* cover the requirements of Protocol View

### Repository for Tool Deployment (I1)

The deployment of tools is a domain that needs to be covered system wide. At the moment,
the tool deployments are attached to certain repository objects, but this connection
will become more superficial once additional entry points to LTI objects for users are
created or system wide reporting or controlling requirements arise. However, the deployments
will need to exist from the beginning.

## Organisation

The plugin is maintained together by [Databay AG](https://www.databay.de) and
[CaT Concepts and Training GmbH](https://www.concepts-and-training.de). PRs
are reviewed, approved and merged by either [Michael Jansen](https://github.com/mjansenDatabay)
or [Richard Klees](https://github.com/klees), preferably the one that is not in
the organisation of the PRs author. For the time being we do not accept PRs from
outsiders.

For test-management the [Testmo platform](https://conceptsandtraining.testmo.net/)
from CaT is used. Test cases will be created and managed by CaT. Issues will be
managed in the [GitHub repository](https://github.com/DatabayAG/ILIAS-LTI-Platform)
of the plugin.

These agreements may change in accordance from both participating parties at any
time.
