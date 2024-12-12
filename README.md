# TEP REST API

This repostory contains the [OpenAPI specification](https://spec.openapis.org/) for the REST interface to transfer data between
[Leolani](https://leolani.github.io/) and the [VRM application]().

## Example Session

| S.No | Action                                                                          | REST call                                                           | Payload                                                                                                                                                                                                                                                                                                                                                                                                                                         | Response                                                                                                                                                                                                                                                                                                 |
|:-----|:--------------------------------------------------------------------------------|:--------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1    | Session start                                                                   | <code>PUT&nbsp;&nbsp;/scenario/ee6--274</code>                      | <details><summary>View JSON</summary><code>{<br>&emsp;"start": "2000-01-01T00:00:00.000+00:00",<br>&emsp;"location": "example.com/ontology/museum/twente",<br>&emsp;"user": "example.com/ontology/alice"<br>}</code>                                                                                                                                                                                                                            | 200<br/><details><summary>View JSON</summary><code>{<br>&emsp;"id": "ee6--274",<br>&emsp;"start": "2000-01-01T00:00:00.000+00:00",<br>&emsp;"location": "example.com/ontology/museum/twente",<br>&emsp;"user": "example.com/ontology/alice"<br>}</code>                                                  |
| 2    | Sara moves to a painting                                                        | <code>POST&nbsp;/scenario/ee6--274/positionchange</code>            | <details><summary>View JSON</summary><code>{<br>&emsp;"previous": {<br>&emsp;&emsp;"x": 0,<br>&emsp;&emsp;"y": 0,<br>&emsp;&emsp;"z": 0<br>&emsp;},<br>&emsp;"current": {<br>&emsp;&emsp;"x": 1,<br>&emsp;&emsp;"y": 2,<br>&emsp;&emsp;"z": 3<br>&emsp;},<br>&emsp;"timestamp": "2000-01-23T04:56:07.000+00:00"<br>}</code></details>                                                                                                           | 200                                                                                                                                                                                                                                                                                                      |
| 3    | Sara starts to look at painting                                                 |                                                                     |                                                                                                                                                                                                                                                                                                                                                                                                                                                 |                                                                                                                                                                                                                                                                                                          |
| 4    | Sara speech: “Who is the artist of the painting?”                               | <code>POST&nbsp;/scenario/ee6--274/audio</code> (running)           | Binary audio stream                                                                                                                                                                                                                                                                                                                                                                                                                             |                                                                                                                                                                                                                                                                                                          |
| 5    | Agent speech: “Hendrik Heerschop”                                               | <code>GET&nbsp;&nbsp;/scenario/ee6--274/chat/response/latest</code> |                                                                                                                                                                                                                                                                                                                                                                                                                                                 | 200<br/><details><summary>View JSON</summary><code>{<br>&emsp;"id": "af1--17c",<br>&emsp;"text": "Hendrik Heerschop"<br>}</code>                                                                                                                                                                         |
|      |                                                                                 | <code>GET&nbsp;&nbsp;/scenario/ee6--274/chat/response/latest</code> |                                                                                                                                                                                                                                                                                                                                                                                                                                                 | 304                                                                                                                                                                                                                                                                                                      |
| 6    | Sara gazes at painting for 5 seconds<br/>(at jar in painting)                   | <code>POST&nbsp;/scenario/ee6--274/gaze</code>                      | <details><summary>View JSON</summary><code>{<br>&emsp;"position": {<br>&emsp;&emsp;"x": 1,<br>&emsp;&emsp;"y": 2,<br>&emsp;&emsp;"z": 3<br>&emsp;},<br>&emsp;"painting": "https://example.com/image.jpg",<br>&emsp;"distance": 1,<br>&emsp;"entities": \[<br>&emsp;&emsp;{<br>&emsp;&emsp;&emsp;"IRI": "https://example.com/ontology/jar"<br>&emsp;&emsp;}<br>&emsp;\],<br>&emsp;"start": "2000-01-23T04:56:07.000+00:00"<br>}</code></details> | 200                                                                                                                                                                                                                                                                                                      |
| 7    | Agent speech: “I see you are quite interested in this painting. May I ask why?” | <code>GET&nbsp;&nbsp;/scenario/ee6--274/chat/response/latest</code> |                                                                                                                                                                                                                                                                                                                                                                                                                                                 | 200<br/><details><summary>View JSON</summary><code>{<br>&emsp;"id": "af1--17c",<br>&emsp;"text": "I see you are quite interested in this painting. May I ask why?"<br>}</code>                                                                                                                           |
| 8    | Sara speech: “I am curious what is in the jar.”                                 | <code>POST&nbsp;/scenario/ee6--274/audio</code> (running)           | Binary audio stream                                                                                                                                                                                                                                                                                                                                                                                                                             |                                                                                                                                                                                                                                                                                                          |
| 9    | Sara gazes at painting for 5 seconds<br/>(at jar in painting) (same as 6)       | <code>POST&nbsp;/scenario/ee6--274/gaze</code>                      | <details><summary>View JSON</summary><code>{<br>&emsp;"position": {<br>&emsp;&emsp;"x": 1,<br>&emsp;&emsp;"y": 2,<br>&emsp;&emsp;"z": 3<br>&emsp;},<br>&emsp;"painting": "https://example.com/image.jpg",<br>&emsp;"distance": 1,<br>&emsp;"entities": \[<br>&emsp;&emsp;{<br>&emsp;&emsp;&emsp;"IRI": "https://example.com/ontology/jar"<br>&emsp;&emsp;}<br>&emsp;\],<br>&emsp;"start": "2000-01-23T04:56:07.000+00:00"<br>}</code></details> | 200                                                                                                                                                                                                                                                                                                      |
| ...  | Interaction continues similar to step 2.-9.                                     |                                                                     |                                                                                                                                                                                                                                                                                                                                                                                                                                                 |                                                                                                                                                                                                                                                                                                          |
| 10   | Session end                                                                     | <code>POST&nbsp;/scenario/ee6--274/stop</code>                      | <details><summary>View JSON</summary><code>"2000-01-23T04:56:07.000+00:00"</code>                                                                                                                                                                                                                                                                                                                                                               | 200<br/><details><summary>View JSON</summary><code>{<br>&emsp;"id": "ee6--274",<br>&emsp;"start": "2000-01-01T00:00:00.000+00:00",<br>&emsp;"end": "2000-01-01T00:10:00.000+00:00",<br>&emsp;"location": "example.com/ontology/museum/twente",<br>&emsp;"user": "example.com/ontology/alice"<br>}</code> |


## Tools

To view and edit the specification you can use the [Swagger Online Editor](https://editor.swagger.io/).
