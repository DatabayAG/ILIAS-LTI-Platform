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
use the [External Concent Plugin](https://github.com/DatabayAG/ExternalContent)
which also implements LTI for ILIAS for a long time. The use of alternative
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
  adhering to clean code principles,  comprehensive documentation, and thorough testing.
  This includes using consistent coding standards, modular design, and automated testing
  to facilitate easy updates and collaboration.
  Overall, this should be an implementation
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
  
## Use-Cases

This section describes the use cases for the plugin as far as they are specific to
the intended LTI platform implementation. Use cases that concern generic ILIAS
functionality, such as granting permission via RBAC or adding options in the repository
of ILIAS are listed here only if there shall be notable deviations from standard
behaviour.

TODO: Read more about:

* [Assignments and Grade Service](https://www.imsglobal.org/spec/lti-ags/v2p0/)
* [Name and Role Provisioning Service](https://www.imsglobal.org/spec/lti-nrps/v2p0)
* [Deep Linking Specification](https://www.imsglobal.org/spec/lti-dl/v2p0)

and include derived use cases here.

### Deployment

* As an **administator** I want to include a LTI tool via the single tenant deployment
  model into my installation.
  * As a **content creator** I want to include LTI tools deployed in the single tenant
    model into my content.
* As an **administrator** I want to include a LTI tool via the multi tenant deployment
  model into my installation.
  * As a **content creator** I want to include LTI tools deployed in the multi tenant
    model into my content.
* As a **content creator** I want to include a LTI tool via the `deployment id as
  account identifier` model by attaching a certain deployment of a tool to my own
  account.
* As a **content creator** I want to include an LTI tool deployed via any model.

### Presentation

* As a **content creator** I want to include an LTI tool as a repository object.
* As a **content creator** I want to include an LTI tool as part of a ILIAS page
  editor page.
* As a **content creator** I want to present/embed an LTI tool directly in ILIAS
  so it appears as normal ILIAS content.
* As a **content creator** I want to be able to forward people to an external page
  that contains the LTI tool.
* As a **content creator** I want to decide which form of presentation (via page
  editor, as "normal" ILIAS content, via external page) is used for a certain LTI
  tool deplyoment.
* As a **learner** I want to have a visual indication of the tool provider and the
  mode of presentation for a certain LTI tool repository object.

### Usage

### Include Tools in the content.

### Monitoring

* As a **tutor** I want to know, which of my learners have already used a tool.
* As a **tutor** I want to have access to the grade book of a tool to have insights
  about the performance of my learners, if the tool supports the
  "Assignment and Grade Services Specification".
* As a **tutor** I want to know the learning progress of my learners for a tool.
* As an **administator** I want to analyze problems of learners with a certain tool.

## Facilities and Views

This section describes the individual parts that make the plugin functionality.
It lists all things that users will find in ILIAS and briefly describes their content
and functionality.

### Plugin

The functionality is delivered via a ILIAS repository object plugin. The plugin
provides the core repository object and allows to add and configure tools. It also
contains the code required for the auxiliary page component plugin.

#### Plugin Configuration

### Tool Configuration

Via the global screen service, the plugin provides an item for the main menu that
gives access to a configuration screen for tools provided in the single and multi
tenant deployment modes.

### Repository Object

#### Creation Screen

#### Settings Tab

The settings screen offers the following standard ILIAS components:

* properties of ilObject (title, description)
* availability (on-/offline, period)

TODO:
* @klees: Do we want to tie icons to the *object* or to the *tool*?
* @mjansenDatabay: IMO the icon should be tied to the *tool*, **not** to the *object*.

#### Participants Tab

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

#### Grade Book, Results and Scores

The "Grade Book, Results and Scores" tab provides a view to the performance of the
users that have interacted with the tool. It is visible/accessible, if the tool
supports the "Assignment and Grade Services Specification" and the "write" permission
is granted for the acting user.

Two sub-tabs are presented:

1. Gradebook
2. Score History

##### Grade Book

The "Grade Book" view presents a tabular gradebook/matrix of users (rows) and
their results (the current/last submitted score in each case) ILIAS stored for
the tool's "Line Items" (columns).

@mjansenDatabay: Describe possible filters etc.

A button "Re-calculate Results" is displayed over the gradebook to re-calculate
the results of the users based on the scores provided by the context.

##### Score History

The view presents a table of the following user score attributes for the
"Line Items" of the specific tool in the "Context" of an ILIAS
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

Since a tool may submit multiple scores for a user for each "Line Item", the
view provides the history of score submissions. Foreach user, who interacted
with the tool, multiple rows are shown, one for each score submission.

The folliwing filters are provided:

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

#### Learning Progress Tab

The learning progress tab provides screens to optionally enable and access the
learning progress of ILIAS users of the tool. It is visible/accessible,
if the tool supports the "Assignment and Grade Services Specification".

The learning progress is disabled by default and can be enabled in the "Settings"
(sub-)tab. The settings screen must provide the following modes:

1. Learning Progress is Deactivated (default)
2. Learning Progress is Activated
  * TODO @mjansenDatabay/@klees: What's the name of this mode? How do we derive the
    ILIAS LP based on the provided user results (and scores)?
    * One Option: Each "Line Item" has a `scoreMaximum` attribute, so we
      could (for instance) calculate the progress for each user and "Line Item",
      and finally map this with the help of thresholds
      (e.g. 80 % is mapped to `completed`, and \[x\] % of all
      "Line Items" needs to be `completed`) to a corresponding ILIAS learning
      progress status.
    * Do we really want to provide such complicated logics?
    * What UI elements do we need for such a configuration on this screen?

If the learning progress is enabled, a button "Re-calculate Learning Progress" is
displayed on top of the settings form. A click on the button re-calculates the
learning progress for all users based on the given results.

Furthermore, the following sub-tabs are available if the learning progress is
enabled:

1. Users
2. Summary

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

#### RBAC and Permissions Tab

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

### Page Component

## Data Model

This section describes the data that the plugin needs to manage itself. It does
not describe hooks into data of other ILIAS systems that the plugin uses via interfaces.

### Tool

### Tool Deployment

### Tool Deployment Interaction Log

## Implementation

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
