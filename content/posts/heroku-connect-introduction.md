---
title: "Heroku Connect 입문"
date: 2022-01-03T13:31:00+09:00
draft: false
---

# Heroku Connect 입문.

## Heroku Connect?
- Salesforce와 Heroku Postgres 간의 데이터 연동을 제공하는 Heroku 공식 애드온입니다. 
- Bulk API, SOAP API, Streaming API 등의 Salesforce API를 이용해서 데이터 연동이 이루어집니다.

## 제한 알아두기.
- Heroku Connect에서 Salesforce를 연결하기 위해 사용하는 유저의 경우에는 일정 권한 이상이 필요합니다. 
- Salesforce의 BULK API를 이용하기 위해, 적어도 Salesforce API Version이 v39이상일 필요가 있습니다.
- Heroku Connect는 Heroku Postgres의 Connection Pooling과 호환되지 않습니다. Connection Pooling를 사용해야할 경우에는 [Incompatibility with Heroku Connect](https://devcenter.heroku.com/articles/postgres-connection-pooling#incompatibility-with-heroku-connect)을 참고하시기를 바랍니다.
- [Review apps](https://devcenter.heroku.com/articles/github-integration-review-apps)에서는 생성된 각 Review app에서 수동으로 애드온을 추가해야합니다.
- Heroku Connect는 데이터 동기화에 사용되는 각종 Postgres function들을 `public` 스키마에 작성합니다. 그로인해, `public` 스키마가 Postgres의 search_path에서 제거되거나, Postgres function들이 삭제될 경우에는 정상적으로 데이터 동기를 진행할 수 없게됩니다. `public` 스키마 또는 관련 functions를 찾을 수 없게된 경우 [How do I resolve "function get_xmlbinary() does not exist" errors in Heroku Connect?](https://help.heroku.com/OZ4RPNQS/how-do-i-resolve-function-get_xmlbinary-does-not-exist-errors-in-heroku-connect)를 참고하시기를 바랍니다.
- Heroku Postgres에서 Salesforce로의 데이터 연동은 Postgres Triggers를 사용하여 테이블의 변화를 모니터링해 변경된 내용을 확인후, Salesforce의 SOAP 또는 BULK API를 이용해 Salesforce로 데이터를 연동합니다. Heroku Connect에서 사용되는 테이블에 대해 커스텀 트리거를 작성하는 경우에는 Heroku Connect에서 오작동을 발생시킬 가능성이 있습니다. 
- Heroku Connect에서 Salesforce로 데이터를 연동할 때, Salesforce의 assignment rules은 동작하지 않습니다. Leads, Cases에 대해 default assignment rules을 유효화가 필요한 경우에는 Heroku Support에 티켓을 작성해서 요청해야합니다. 

