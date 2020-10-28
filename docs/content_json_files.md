# Content Entities JSONs

## `content-json-files`

- ### `challenges`

  - split into two folders (`en` and `fr`)

    - for `fr` files, its the same structure but the content where applicable is in french

  - Filename structure: `<locale>-<level>-<type>-<random_hash>.json`

    - where `level` is `Beginner`, `Intermediate`, `Expert`
    - where `type` is `multichoice`, `binary_choice`, `correctCode`, 
    - `en-Beginner-multichoice-f6dfd9e6933549c53bce931c3d20d3b30491d11b-6f4302861497a150fc6a572c019741b027d09930.json`

  - Some files look like this: `<locale>-<language>-<level>-<type>-<random_hash>.json`

    - so language is a precursor to `level`. Also language can be `android` 
    - `en-android-Beginner-multichoice-36537ffbe2507e76d577df65afa3d5be6919ba18-7be3de598acc49db5225d7fc7e0ce4e7a168de10.json`

  - `answer_type` field is either an array of numbered strings OR its a singular numerical string. (Even for singular answers, it goes back and forth)

  - `source_code` field inside `data_json.user_data` can appear singular, for a multichoice question that has a block of code

  - Looks like files some files have references rather than actual content

    - This is probs references to translations

  - Beginner Android MultiChoice:

    ```json
    {
      "list": [
        {
          "class_name": "Content::Challenge",
          "id": 123,
          "document_shasum": "",
          "data_json_shasum": "",
          "author": "email",
          "language": null,
          "level": "Beginner",
          "type": "multichoice",
          "data_json": {
          	"user_data": {
          		"question": "",
          		"answers": [],
        		},
    				"user_feedback": {
              "2": "There are different ways to protect from injection attacks..."
            }
        	},
          "status": "looks like this is empty or null all the time",
          "tags_json": [
            "CSInjection",
            "EOL",
            "non-auto-generated"
            ...
          ],
          "deleted_at": "timestamp",
          "created_at": "",
          "updated_at": "",
          "title": "How to protect...",
          "correct_answer": "2",
          "checked_out_user": "looks like its null",
          "answer_type": "single",
          "locale": "en",
          "content_tags": {
            "vulnerabilities": [{
              "name": "vulnerability:client_side_injection",
              "display_name": "Client Side Injection"          
            	}
            ],
            "appsec_categories": [{
              "name": "appsec_category:sensitive_data_exposure",
              "display_name": "...",
            	}
            ],
           	"programming_languages": [
              {
                "name": "programming_language:ios",
                "display_name": "..."
              },
              {
                "name": "programming_language:android",
                "display_name": "...",
              }
            ]
          },
          "name": "filename without extension",
          "enabled_for_tournaments": "true",
          "time_to_answer_secs": 60
        }
      ]
    }
    ```

  - Expert CorrectCode

    ``` json
    {
      "level": "Expert",
      "type": "correctCode",
      "data_json": {
        "code_mirror_mode": "text/x-c++src", // Was undefined for several files, this is the language of the question
        "user_data": {
          "question": "...",
          "source_code1": "<% ..... /%>",
          "source_code2": "<% ..... /%>", 
          // ...
        },
        "user_feedback": {}
      },
      "correct_answer": "[\"1\"]",
    }
    ```

  - Beginner Multicode (no programming language). References are probs translations

    ```json
    {
      "title": "{{challenge_83edb561bfcad35bcd62492cdf151b51c2562ecd}}",
      "data_json": {
        "user_data": {
          "answers": [
            "{{challenge_bc75b280a8d069629a4597eec2e8032e601f8d43}}",
            "{{challenge_956427e76e46c92b8101ff99a8572178a179e572}}",
            "{{challenge_613325c123292bd4fe8be87ec137fcdb64c13e36}}",
            "{{challenge_c091107896212fb82347cc3a1f256c665c8ec021}}"
          ],
          "question": "{{challenge_83edb561bfcad35bcd62492cdf151b51c2562ecd}}"
        },
        "user_feedback": {
        	"3": "{{challenge_60baee5afe2bad1a688018aac73e90d56302b830}}"
        },
        "isSavedWithKeys": true
      }
    }
    ```

    ---

    

- ### `lessons`

  - split into `en` and `fr`

  - Filename structure: `lesson_<programming_language>_<vulnerability>_<locale>.json`

    - `lesson_angular_js_dom_open_redirect_angular_en.json`
    - guessing that vulnerability or the combination of pl and vuln is actually the course name?

  - `lessonConfig` field looks like what the frontend code actually takes and builds the course by

  - Angular Anti XSS Mechanism HTML Sanitizer

    ```json
    {
      "list": [
        {
          "class_name": "Content::Lesson",
          "data_json": {
            "steps": [
            	{
            		"title": '{{lesson_cc9086c81c70ccebf760ea704977aa6548fe7e59}}',
                "content": [
                  {
                    "content": "{{lesson_b5bdcf492355e38af1167e4b60fe3bea0e650259}}",
                    "action": "{{lesson_da0b8879b7333f67d0661b6452d8ac2d75775a45}}" 
                  }
                ]
          		},
              {
                "title": "...",
                "content": [
                	{
                		"content": "...",
                    "action": "..."
              		}
                ]
              },
              {
                //...
              }
            ],
            "layout": {
              "orientation": "vertival",
              "items": [
                {
                  "type": "accordion-stepper",
                  "name": "stepper",
                  //...
                },
                {
                  //...
                }
              ]
            },
            "lessonConfig": {
              "steps": [
                {
                  "activeIndex": -1,
                  "components": {
                    "stepper": {
                      "state": {
                        "steps": {
                          "linkedTo": "steps"
                        },
                        "button": {
                          "title": "NEXT"
                        }
                      }
                    }
                  }
                },
                {
                  "activeIndex": 0,
                  "subStep": 0,
                  "components": {
                    //...
                  }
                }
              ]
            }
          },
    			"editByNewEditor": true,
          "allowSkipSteps": true,
          "isSavedWithKeys": true,
          "useUserLocale": true,
          "summary": [
                 "{{lesson_9395daa6d5f33062cf304185041b6fcf98afecfe}}",
                 "{{lesson_44ef26ac3b95b48ea646ccc07cc61ded61ba02df}}",
                 "{{lesson_cd79662884e50a02d749bcb4b68e1a9faf9c537e}}",
                 "{{lesson_f767892cde0aa81daf1c4c949d330117240cc0aa}}",
                 "{{lesson_dc0e20350d52a4be2c2c2b183c6aa782b6b0efd8}}"
             ],
          "intro": "{{lesson_83692b24f6c62e536c4e2f190553ad0cbb1cbb50}}"
          },
          "tags_json": [],
          "language": null,
          "course_id": null, //is numeric in several files
          "author": "adili.alon@checkmarx.aws",
          "id": 968,
          "locale": "en",
          "name": "angular_anti_xss_mechanism_html_sanitaizer",
          "display_name": "Angular anti XSS mechanism - html sanitizer",
          "challenge_query": null,
          "checked_out_user": null,
          "allowed_on_free_platform_flavor": false,
          "lesson_type": "normal",
          "document_shasum": "55aa263bcd10b528a63bb7bb94a7784d6fe57c24",
          "data_json_shasum": "7e6a884fcc1bad6ff0b65118b8bf63239c1eb5e8",
          "deleted_at": null,
          "created_at": "2020-06-14T13:06:29.000Z",
          "updated_at": "2020-08-30T07:29:37.000Z"
        }
      ]
    }
    ```

  - Cross Site Request Forgery Post Perl Java Perl Java En

    ```json
    {
      "data_json": {
        "variabels": null, //not a typo on my part
        "baseLesson": {
          "class_name": "Content::Lesson",
          "course_id": 6,
          "challenge_query": "tbd",
          // ...
          // Its essentially a copy paste of another lesson into this one (instead of a reference)
        }
      }
    }
    ```

    ---

    

- ### `translations`

  - Two json files: `en` and `fr`

  - each json is a map between some key and a string

  - ```json
    {
          "lesson_7a7620a87051eb9077a7c7185309f9839e813092": "<p>In this interactive tutorial we will demonstrate how DOM-based XSS attacks can compromise the security of a web application, and how to write code more securely to protect against this type of attacks.</p>",
        "lesson_d5c4657ee5c6167a4d9ac4e03f389ba74e8ec4a6": "<div>In this module, we’ve learned how the Document Object Model Cross-Site Scripting (DOM XSS) attacks work.</div>",
        "lesson_3332d1382deda280218594d13419dfff47c02bb8": "<div>We’ve also learned that writing secure code to protect against DOM XSS is a more difficult task than writing secure code to prevent Persistent XSS and Stored XSS attacks, due to the much larger attack surface with DOM XSS.</div>",
         "Delete": "Delete",
        "EmptyList.Add new link": "Add new link",
        "EmptyList.Click the above button to be the first one to share": "Click the above button to be the first one to share.",
        "EmptyList.No one has posted anything yet": "No one has posted anything yet",
        "EndLessonControlPanel.End Lesson": "End Lesson",
        "EndLessonControlPanel.Play Again": "Play Again",
        "Like": "Like",
    }
    ```

    ---

    

- ### `walkthroughs`

  - Filename structure: `walkthrough_<subject/vulnerability>_<programming_language>_<locale>.json`

    - `walkthrough_cross_site_request_forgery_post3_python_en.json`

  - Walthrough 2SQLI Java Dotnet

    ```json
    {
      "list": [
        {
          "class_name": "Content::Walkthrough",
          "id": 3341,
          "author": "email",
          "language": "dotnet",
          "tags_json": [],
          "status": null,
          "data_json": {
            "user_data": {
              "title": "2SQLi-java-01",
              "code_behavior": "tooltips",
             	"snippets": [
                {
                  "txt": "public class TransferRepository ....",
                  "tooltips": [
                    {
                      "id": 0,
                      "text": "<code>...</code>",
                      "position": {
                        "line": 0,
                        "ch": 40
                      }
                    }
                  ],
                  "fixCommands": null,
                  "fixedToolTips": null,
                  "connectToStep": null,
                  "markWord": null,
                  "specifiedLines": null,
                  "initialCode": "public class TransferRepository ...",
                  "fixed_code": "",
                  "code_mirror_mode": "dotnet",
                  "name": "2SQLi-java-01",
                  "position": 0,
                  "sha": "82e77fcda3a2b0ca34918902e6f075bdfba8fc90"
                }
              ]
            }
          },
          "created_at": "2019-09-04T16:29:53.000Z",
          "updated_at": "2019-09-08T07:21:36.000Z",
          "checked_out_user": null,
          "document_shasum": "f7736f75a43e4f9afa99c9dfc0266bf0e65b1cb0",
          "data_json_shasum": "14ee63ec38062f7da6d8944709f2eff96fee0428",
          "locale": "en",
          "allowed_on_free_platform_flavor": false,
          "deleted_at": null
        }
      ]
    }
    ```

  - 