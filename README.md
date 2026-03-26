Part-1
Question-1

	Resource modules are reusable and not fully configured declarative modules that specify the desired state of a part of an environment(e.g.). They can be made public because they are generic and do not contain proprietary information

-Pattern modules are a collection of resource modules that are already configured to create a specific environment. They may contain company-sensitive information about:
-Company best practices
-Architecture decisions
-Optimizations
-Internal structure and design patterns

Question-2

	WAM-modules are designed to embed Well-Architected Framework (WAF) principles by default, meaning each module enforces security, reliability, and operational best practices through predefined configurations. For example, a Key Vault module would enforce the security pillar by:
•  Disabling public network access 
•  Enabling RBAC authorization 
•  Restricting access via private endpoints 
•  Enforcing access policies and soft delete



Part-2
Question-1
1.	The developer asks for a specific environment to the AI IDE.
2.	AI connects to the MCP server what resource/pattern modules exist and what inputs they need
3.	AI picks modules and generates terraform
4.	AI uses terraform plan to preview changes
5.	Dev approval decides if the workflow continues
6.	CI/CD pipeline executes after validating and enforcing policies








Question-2 
	Ai models can sometimes hallucinate. This is why we provide them with external and constantly updated context. This is achieved through using a RAG approach supported by an ingestion pipeline. WAM-modules are stored in private repositories and are ingested offline. Ingestion gets the data processed, structured and indexed in a database to enable fast and relevant retrieval. At runtime the AI does not directly access the database but interacts with the MCP server, which exposes controlled tools such as retrieval modules and their required inputs. This way the MCP server uses the retrieval system (RAG) to fetch the correct and up-to-date context to the ai. The retrieval system ensures that the AI always works with authoriative data instead of hallucinating while ingestion makes this retrieval process efficient and scalable.


Question-3
	With a custom MCP-server that lets ai use terraform commands. It would pose real security and stability threats to the systems to let AI use terraform apply directly without the developer’s control. However, the AI can safely use terraform apply to show the compare the code and see what will change after terraform apply. When doing critical changes, AI assistance where the developer makes the final decision is a better option than automated execution where the ai acts on its own without the dev’s permission or control.



Part-3


Question-1
We would ensure the use of right variable input and naming conventions by using terraform validation blocks and add linters and policy checks like tflint and tfsec. we would also let CI checks control the naming to match the regex patterns which would only allow certain ranges of characters






Question-2
When an AI agent updates   a Wam resource module, it should create a pull request in the version control system instead of directly deploying the changes. This ensures versioned, reviewable and automatically validated changes. Once PR is created, the CI/CD pipeline performs multi-tiered testing:
-unit tests validate the internal logic of the module, variable definitions and expected outputs.
-integration tests verify that the module works correctly when combined with other tests
-End-to-End tests deploy the module in a sandbox environment to confirm that it would work in a real-world scenario.

The AI monitors pipeline results and use the feedback to improve its code. This becomes a loop until the code works perfectly and all checks passes.


Question-3
 An MCP server is lets AI use tools and functions through API’s. this is powerful and valuable but also vulnerable. It is built for functionality not security. It can do things like querying customer databases, so what could stop it from pulling all the sales records instead of the numbers it needs. Or It’s access to commanding over an entire infra could be maliciously taken advantages by an attacker. Along with all its capability to make changes in the infra it also has data of things like proprietary business logic. The wide access of the MCP server is an attractive target for attackers.

	These problems could be mitigated by closing the MCP server to access from outside the private network and very strictly logged inside the private network. The inputs should be validated and filtered to prevent prompt-injection. Certain functions and tools that AI has access to through the MCP server could be limited to certain roles in the team. Furthermore, the AI agent should not have more privileges and/or access to data than it needs. The rest of the protection would come from basic firewall protections:
-Packets always being encrypted.
-Suspicious or unusual packets being dropped.

