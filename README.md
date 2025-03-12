# dify api [completion/chat/workflow] Public interface traversable
dify api [completion/chat/workflow] There is a public interface traversal vulnerability. Unauthorized attackers can obtain device information by traversing the 16-bit API path and use the newly created model application.Unauthorized attackers can use the specific path behind the [completion/chat/workflow] API as the application identifier of the model API. After successfully obtaining the API, they can use the model application created by the Dify content creator through the traversed API, and can use the model resources of the Dify content creator without restriction, and even the physical host resources of the model.
# What is Dify 
is an open-source LLM app development platform. Dify's intuitive interface combines AI workflow, RAG pipeline, agent capabilities, model management, observability features and more, letting you quickly go from prototype to production.https://github.com/langgenius/dify

# Dify Public API
![image](https://github.com/happy0717/dify_api_-completion-chat-workflow-_Public_interface_traversable/blob/main/img/Snipaste_2025-03-12_17-51-28.png)
![image](https://github.com/happy0717/dify_api_-completion-chat-workflow-_Public_interface_traversable/blob/main/img/Snipaste_2025-03-12_17-55-34.png)
host/workflow/[A-Z0-9a-z]{16}


host/chat/[A-Z0-9a-z]{16}


host/completion/[A-Z0-9a-z]{16}


As can be seen from the screenshot, no authorization information is required to access the API like  [completion/chat/workflow].

According to the official documentation(https://docs.dify.ai/guides/application-publishing/launch-your-webapp-quickly), the API generated here will be published publicly on the internet even if it is released, and there is no place to set authentication on the page.
![image](https://github.com/happy0717/dify_api_-completion-chat-workflow-_Public_interface_traversable/blob/main/img/Snipaste_2025-03-12_18-00-24.png)
In addition, there is no prompt to users on the model application creation page, and there is no clear indication that the created model application will be public. It turns out that most users do not know that the newly created model application is public.
# Risk Description

The risk here can correspond to OWASP Top 10 API Security Risks – 2023：

1.API5:2023 - Broken Function Level Authorization 

2.API6:2023 - Unrestricted Access to Sensitive Business Flows 

3.API4:2023 - Unrestricted Resource Consumption

Unauthorized attackers can use the specific path behind the [completion/chat/workflow] API as the application identifier of the model API. After successfully obtaining the API, they can use the model application created by the Dify content creator through the traversed API, and can use the model resources of the Dify content creator without restriction, and even the physical host resources of the model.
# Why are model application parameters traversable?

In the Dify code api\events\event_handlers\create_site_record_when_app_created.py, there is a function handle (Create site record when an app is created). In this function, generate_code is called and a specified length of 16 is passed. Then, 16 characters are randomly selected from uppercase letters, lowercase letters and numbers as the model application identifier through Python's random.choice. By splicing these model application identifiers after the API [completion/chat/workflow], the model application can be accessed without authorization.
![image](https://github.com/happy0717/dify_api_-completion-chat-workflow-_Public_interface_traversable/blob/main/img/Snipaste_2025-03-12_18-11-43.png)
![image](https://github.com/happy0717/dify_api_-completion-chat-workflow-_Public_interface_traversable/blob/main/img/Snipaste_2025-03-12_18-11-55.png)
![image](http://github.com/happy0717/dify_api_-completion-chat-workflow-_Public_interface_traversable/blob/main/img/Snipaste_2025-03-12_18-12-11.png)
![image](https://github.com/happy0717/dify_api_-completion-chat-workflow-_Public_interface_traversable/blob/main/img/Snipaste_2025-03-12_18-12-31.png)
# Repair method

1.Increasing the length of the random path of the model application API, such as 64 or 128 bits, can increase the possibility of successful brute force guessing by malicious attackers.
2.Added an authentication switch for the [completion/chat/workflow] interface on the page, allowing users to choose whether to publicly release the model application
3.Remind users on the page that newly created model applications will be made public
