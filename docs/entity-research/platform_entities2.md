---
layout: default
title: Platform Content Entities Different Format
parent: Entity Research
nav_order: 2
---

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

- [Relationship Diagram](#relationship-diagram)
- [`challenge`](#challenge)
  - [`models/content/challenge`](#modelscontentchallenge)
  - [`models/challenge`](#modelschallenge)
- [`course`](#course)
  - [`models/content/course`](#modelscontentcourse)
  - [`models/content/definitions/course`](#modelscontentdefinitionscourse)
  - [`models/v2/course`](#modelsv2course)
- [`game`](#game)
  - [`models/content/game`](#modelscontentgame)
  - [`models/game`](#modelsgame)
- [`lesson`](#lesson)
  - [`models/content/lesson`](#modelscontentlesson)
  - [`models/content/definitions/lesson`](#modelscontentdefinitionslesson)
  - [`models/v2/lesson`](#modelsv2lesson)
- [`programming_language`](#programming_language)
  - [`models/content/programming_language`](#modelscontentprogramming_language)
- [`tag`](#tag)
  - [`models/content/tag`](#modelscontenttag)
- [`vulnerability_mapping`](#vulnerability_mapping)
  - [`models/content/vulnerabilty_mapping`](#modelscontentvulnerabilty_mapping)
- [`walkthrough`](#walkthrough)
  - [`models/content/walkthrough`](#modelscontentwalkthrough)
- [`global_version`](#global_version)
  - [`models/global_version`](#modelsglobal_version)
- [`learning_resource`](#learning_resource)
  - [`models/learning_resource`](#modelslearning_resource)
- [`learning_resource/link`](#learning_resourcelink)
  - [`models/learning_resource/link`](#modelslearning_resourcelink)
- [`tagged_lesson`](#tagged_lesson)
  - [`models/tagged_lesson`](#modelstagged_lesson)
- [`vulnerability_tag`](#vulnerability_tag)
  - [`models/vulnerability_tag`](#modelsvulnerability_tag)
- [`custom_course_lesson`](#custom_course_lesson)
  - [`models/v2/custom_course_lesson`](#modelsv2custom_course_lesson)
- [`cms_log`](#cms_log)
  - [`models/content/cms_log`](#modelscontentcms_log)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->
--- 

## Relationship Diagram
 ![Entity Diagram]({{ site.baseurl }}/assets/images/platform_entities_diagram.png)


## `challenge`

### `models/content/challenge`

- A specific challenge, used both in platform and in the tournaments
- Table Name: `content_challenges`
- paranoid
- has many: `challenges` as in `models/challenge`
- has and belongs to many: `programming_languages`
- has a difficulty level
- has a `type`:  ??
- has `content_tags`, `programming_language_tags`, looks like internally it has `appsec_category_tag` and `vulnerability_tag`
- creates new `ProgrammingLanguages` when unfamiliar tags are sent

  

### `models/challenge`

- Basically models a users challenge attempt, it saves the points, number of attempts, submitted answers, the challenge type and if correct or not
- belongs_to: `user`, `course(V2::Course)`, `lesson(V2::Lesson)`, `challengeable`, `practice`, `game`, `content_challenge`
- has_one: `score` with foreign_key: `meritable_id`
- paranoid
- has a state machine to determine challenge status (`started`, `completed`)
- has also an enum mapping for game level (`beginner`, `intermediate`, `advanced`)
  

## `course`

### `models/content/course`

- Models the course
- Table Name: `content_courses`
- has_many: `lessons`



### `models/content/definitions/course`

- Much more robust than `content/course`
- This is for customers wanting to add a course by themselves, aka custom course
- Table Name: `content_definition_courses`
- has_many: `content_definition_lessons`
- has_many: `update_versions`, class name is `GlobalVersion`



### `models/v2/course`

- This is an instance of a specifc course set up for the tenant. This is what each user sees when he looks at available courses
- Table_name: `v2_courses`
- has_many: `lessons (v2::Lessons)`, `practices`,  `challenges`,  `primary_users` (Which is basically the `User` class. This basically sets up the linkage of `primary_course`),  `tagged_lessons`,  `custom_course_lesson`,  `user_course_progress`,  `primary_teams`, class name is `Team`
- has an `active` attribute
- has an `active_on_free` attribute.
- For some reason has several constants which are like tags that specify which courses are relevant to which subject
  - `FRONTEND_COURSES = %w(frontend http microlessons)`
  - `MOBILE_COURSES = %w(ios android)`
  - etc. !?
  - This seems to be used when creating an external link **for a lesson**
    - Also looks like there is a public bucket and a production bucket
- paranoid


## `game`

### `models/content/game`

- A collection of challenges, built based on a challenge query
- Table Name: `content_games`
- name has to be unique
- number of questions has to be  `0 <= x <= 30`
- has a challenge query(?), a language, a level
- *Level is `beginner/intermediate/expert`* 
- Language is taken from `::Content::Tag.all_programming_lanauge_tags`


### `models/game`

- Most probably an instance of a game, or a custom game per tenant
- belongs_to: `user`, `content_game(Content::Game)`
- has_many: `challenges`
- has_one: `score` with foreign_key: `meritable_id`
- paranoid
- has to have a `content_game` relation
- has a state machine to determine the game status (`started`, `completed`)

  

## `lesson`

### `models/content/lesson`

- Models a lesson which is part of a `course`
- Table Name: `content_lessons`
- A course **name** has to be unique for a specific **locale**
- Has a name and a display_name
- Has a boolean stating if its available in codebashing free or not
- has a lesson type with default `normal`, in the Staging DB its the only value



### `models/content/definitions/lesson`

- Table Name: `content_definition_lesosns`
- belongs_to: `content_definition_course`
- has_many: `update_versions`



### `models/v2/lesson`

- Table_name: `v2_lessons`
- belongs_to: `course`
- has_many: `practices`, `learning_resources`, `challenges`, `tagged_lessons`, `custom_course_lessons`
- has a `minimum_compeltion_time`
- has a boolean to set if its available for `free` or not
- has an `external_link` attribute
- has an `active` attribute, which means a lesson can be inactive too



## `programming_language`

### `models/content/programming_language`

- Records for programming languages
- Table Name: `programming_languages`
- Has a display name and a name which both have to be unique
- Has a boolean to indicate if its available for tournaments or not
- Has and belongs to many: `content_challenges`



## `tag`

### `models/content/tag`

- These are rows of all the possible tags that can be added to content
- Table Name: `content_tags`
- Has a validation regex
- refactors the tags to be of format `tag_context:tag_name`
- Uses `tag_context`to differentiate between tag groups
- `appsec_categories` is the de-facto tag for displaying stuff, as in, its the default category
- `appsec_category`, `programming_language`, `vulnerability`



## `vulnerability_mapping`

### `models/content/vulnerabilty_mapping`

- This maps a vulnerability to a programming language, lesson_name and course_name
- Table Name: `vulnerabilty_mappings`
- belongs_to: `programming_language`
- Absurdly, this seems to be used to find a course's language



## `walkthrough`

### `models/content/walkthrough`

- ??? - Looks similar to `challenge` though
- Table Name: `content_walkthroughs`
- paranoid

  

## `global_version`

### `models/global_version`

- part of `PaperTrail`
- Looks like this is used as some entity versioning scheme, most importantly this looks like it saves the `object_changes` so technically its possible to navigate between versions



## `learning_resource`

### `models/learning_resource`

- This looks to be the table where learning resources are saved for every tenant
- belongs_to: `user`, `lesson`
- has_many: `impressions`(as `imppressionable`) and its dependency is destroy
- There is a PER TENANT limit of 1000(?!)
- Has the ability to save votes
- is linked to `user`, `license`, and lesson.
- has an `email` field
- has a boolean to state if its a cb resource or not
- has a `type`, `title`, `description` and `json_payload`
- has a method which creates getters and setters based on the json payload (so its a generic implementation)

  

## `learning_resource/link`

### `models/learning_resource/link`

- This actually inherits `models/learning_resource` so its basically a specific `resource_learning` type
- Looks like this is STI
- type = `link`
- Basically this jsonifies the url and the object into a json which is saved in the `json_payload` column
- `title` has to be less than 30 chars
- `description` has to be less than 50 chars

  

## `tagged_lesson`

### `models/tagged_lesson`

- Looks like this is actually assigned lesson
- belongs_to: `language` but with class name `V2::Course` ?!
- belongs_to: `lesson` 
- belongs_to: `user`
- has to have a all associations and also a `created_via` which is an enum(`api` or `ui`)

  

## `vulnerability_tag`

### `models/vulnerability_tag`

- has a `cwe_id` and `cwe_link`
- has a `owasp_id` and `wasp_link`
- has a `sans_id` and `sans_link`
- is linked to a `lesson` name (which lesson though? `v2_lesson?!` )

  

## `custom_course_lesson`

### `models/v2/custom_course_lesson`

- Models a lesson for a custom course, including the order
- Table_name: `v2_custom_course_lessons`
- belongs_to: `course`
- belongs_to: `lesson` 



## `cms_log`

### `models/content/cms_log`

- Looks like this is for saving the logs from the CMS for creation or some stuff
- Table Name: `cms_logs`
