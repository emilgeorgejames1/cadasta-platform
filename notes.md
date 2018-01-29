We can see that all projects have valid `current_questionnaire` values or `None` or `''`:

```py
In [6]: Project.objects.exclude(current_questionnaire__in=Questionnaire.objects.values_list('id', fl
   ...: at=True)).exclude(current_questionnaire=None).exclude(current_questionnaire='').count()
(0.001) SELECT COUNT(*) AS "__count" FROM "organization_project" WHERE (NOT ("organization_project"."current_questionnaire" IN (SELECT U0."id" AS Col1 FROM "questionnaires_questionnaire" U0) AND "organization_project"."current_questionnaire" IS NOT NULL) AND NOT ("organization_project"."current_questionnaire" IS NULL) AND NOT ("organization_project"."current_questionnaire" = '' AND "organization_project"."current_questionnaire" IS NOT NULL)); args=('',)
DEBUG:django.db.backends:(0.001) SELECT COUNT(*) AS "__count" FROM "organization_project" WHERE (NOT ("organization_project"."current_questionnaire" IN (SELECT U0."id" AS Col1 FROM "questionnaires_questionnaire" U0) AND "organization_project"."current_questionnaire" IS NOT NULL) AND NOT ("organization_project"."current_questionnaire" IS NULL) AND NOT ("organization_project"."current_questionnaire" = '' AND "organization_project"."current_questionnaire" IS NOT NULL)); args=('',)
Out[6]: 0

In [7]: Project.objects.filter(current_questionnaire='').count()
(0.002) SELECT COUNT(*) AS "__count" FROM "organization_project" WHERE "organization_project"."current_questionnaire" = ''; args=('',)
DEBUG:django.db.backends:(0.002) SELECT COUNT(*) AS "__count" FROM "organization_project" WHERE "organization_project"."current_questionnaire" = ''; args=('',)
Out[7]: 26

In [8]: Project.objects.filter(current_questionnaire=None).count()
(0.001) SELECT COUNT(*) AS "__count" FROM "organization_project" WHERE "organization_project"."current_questionnaire" IS NULL; args=()
DEBUG:django.db.backends:(0.001) SELECT COUNT(*) AS "__count" FROM "organization_project" WHERE "organization_project"."current_questionnaire" IS NULL; args=()
Out[8]: 133
```

1. Clear any Project's `current_questionnaire` value if there is no Questionnaire by that `id`

2. Delete all questionnaires that are not the `current_questionnaire` for any project.
