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
  dependency injection as much as possible. Overall, this should be an implementation
  that looks and feels like ILIAS, in the frontend and the backend, as much as possible.
* The implementation should provide a basis to create specialized implementations
  for specific LTI 1.3 tools.

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

## Facilities and Views

## Data Model

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
