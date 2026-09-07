# DevOps and Cloud Interview Guide

## Questions and Assignments Provided

> This file transcribes the course items visible in the screenshots provided by the user. It is not a complete copy of the paid course.

### Section 2: Git Basics

4. Git Fork vs Git Clone

5. Explain a scenario where you used Git Fork instead of Git Clone.

6. Git Fork in action with example

**Assignment 1:** Create a fork of the project mentioned below and create a pull request.

7. Git Fetch vs Git Pull

8. Show how Git Fetch and Git Pull work in realtime.

9. Which command do you use mostly: Git Fetch or Git Pull, and why?

**Assignment 2:** Practice Git Fetch vs Git Pull on a GitHub repository.

10. Git Rebase vs Git Merge: detailed explanation

11. Show practically how Git Rebase is different from Git Merge.

12. How to explain Git Merge vs Git Rebase in interviews (short explanation)

**Assignment 3:** Practice Git Merge vs Git Rebase.

13. Explain the Git branching strategy that you used in your company.

14. Explain 3 challenges that you faced with Git during your work experience.

15. Explain the recent challenge that you faced with Git and how did you address it.

16. How do you handle merge conflicts in Git?

17. Explain Git merge strategies: Ours and Theirs strategies.

18. Have you ever used Git tags? If yes, why?

19. How do you combine multiple commits into a single commit?

20. Explain 10 Git commands that you use on a day-to-day basis.

21. I want to ignore pushing changes to a file to Git. How can you do it?

22. What is the purpose of the `.git` folder in a Git repository?

23. Can you restore a deleted `.git` folder?

24. A teammate accidentally committed a Kubernetes Secret (base64 encoded) to Git.

## Section 3: Linux

25. 10 Linux commands that you use on a day-to-day basis.

26. Can you restore a lost PEM file? If not, how can you access the instance?

27. `/var` is almost 90 percent full. What will be your next steps?

28. Linux Server is slow due to high CPU utilization. How will you fix it?

29. Application deployed on Nginx returns Connection Refused. How will you fix it?

30. SSH to an instance stopped working. How will you troubleshoot the issue?

31. Find and list the log files older than 7 days in the `/var/log` folder.

32. Find and remove the log files older than 30 days in a folder.

33. Cronjob + shell script to perform advanced log rotation (scenario provided).

34. Bulk creation of Linux users using a CSV file.

35. Service health monitor script in Bash.

36. Find and delete files over 100MB.

37. Get the list of users who logged in today (scenario: some packages deleted).

38. Website doesn't load. How will you investigate?

39. Using the `sed` command, how do you remove the first and last line of the file?

40. What are the different types of variables in Linux?

41. Kill vs Kill -9 in Linux.

## Section 4: Networking

42. Explain DNS in simple words.

43. Explain the complete flow of the request from client to server (OSI Model).

44. Explain the difference between Forward Proxy and Reverse Proxy.

45. User reports slowness in the app. How would you approach this?

46. Curl works with IP, fails with domain. Why?

47. Website returns 502 HTTP Status code. What can be the issue?

48. [Many people can't answer] What is the difference between `0.0.0.0` and `127.0.0.1`?

49. What is the difference between Public and Private Subnets?

50. You accidentally created a private subnet instead of public. How will you fix it?

## Section 5: CI/CD

51. What are Jenkins shared libraries and how do they work?

52. Talk about 5 build targets that you use on a day-to-day basis.

53. Which artifact repository do you use for builds?

54. How do you configure Artifactory for your application in Maven?

55. Build passed locally but fails in CI. How will you troubleshoot?

56. CI pipeline succeeds but the app is broken in production. What action will you take?

57. Pipeline slows down over time (builds taking more time). How will you fix it?

58. A developer pushes a feature branch, but the pipeline doesn't trigger.

59. Your build fails because it can't download a dependency from your artifact repos.

60. Python build fails on CI but works locally. What can be the issue?

61. Explain the Python application build process in detail.

62. Using static code analysis, what kind of problems can you identify?

63. Static code analysis slows down the CI pipeline. How will you fix it?

64. App in 'OutOfSync' state in Argo CD, but no Git changes.

65. When a build fails in Jenkins, how will you send an email?

## Section 6: Terraform

66. What is the difference between `for_each` and `for` in Terraform?

67. What are modules in Terraform and why should we use them?

68. What is the role of the statefile in Terraform?

69. Have you considered storing statefile in Git instead of AWS S3 or Azure Blob?

70. Explain Terraform statefile management.

71. Two DevOps Engineers attempt to update the statefile at once. What happens?

72. We don't have a cloud account. Where can we store the statefile?

73. Do you use Terraform Enterprise or Community version?

74. Have you heard about OpenTofu? Do you think it is better than Terraform?

75. Write Terraform code to create any resource on AWS.

76. What is the difference between Resource and Datasource in Terraform?

## Section 7: Docker

77. Docker container exits immediately. How will you troubleshoot?

78. [90 percent get this wrong] What is the purpose of `EXPOSE` in a Dockerfile?

79. Port is not accessible on localhost even after port mapping in Docker.

80. Data is lost when a container stops and restarts. How will you fix it?

81. You made a change in your code, rebuilt the image, but the change isn't reflected.

82. App crashes with "Permission Denied" in a container but works fine on localhost.

83. Docker host is running out of disk space. How do you clean up?

84. How will you debug a live container?

85. Which container registry do you use in your organization?

86. Explain the difference between `CMD` and `ENTRYPOINT` in Docker.

87. What Docker commands do you use on a day-to-day basis?

88. When will you forcefully remove a container and how?

## Section 8: Kubernetes

89. Explain Kubernetes cluster architecture.

90. How do various components of Kubernetes interact when you run `kubectl apply` (Pod)?

91. What is the purpose of Services in Kubernetes?

92. Why is hardcoding Pod IP communication a bad practice?

93. What are the types of Services in Kubernetes?

94. What are labels and selectors in Kubernetes?

95. What would you recommend: NodePort Service or LoadBalancer-type Service, and why?

96. How are Kubernetes Services related to Kube Proxy?

97. What is the disadvantage of LoadBalancer Service type?

98. What is a Headless Service in Kubernetes and when did you use it?

99. Can a Pod access a Service in a different namespace? If yes, how?

100. Explain how you can restrict access to a database Pod to only one app in the namespace.

101. Explain the deployment strategy that you follow in your organization.

102. Explain the rollback strategy that you follow in your organization.

103. Design a solution to avoid rollbacks.

104. Explain the deployment strategies that you used in the past.

105. Explain the role of CoreDNS in Kubernetes.

106. A DevOps engineer tainted a node as "Noschedule". Can you still schedule a Pod?

107. Pod is stuck in CrashLoopBackOff. What steps will you take?

108. What is the difference between liveness and readiness probes?

109. Explain the difference between Ingress and LoadBalancer Service type.

110. Your app works with ClusterIP but fails with Ingress. How do you troubleshoot it?

111. Why do I need to set up an Ingress controller after creating Ingress?

112. We have an in-house load balancer. Can we use Ingress with our load balancer?

113. Your Deployment has replicas: 3, but only 1 Pod is running. What could be wrong?

114. Your Pod mounts a ConfigMap, but changes to the ConfigMap are not reflected.

115. Explain how Node Affinity works and when will you use it?

116. What is the difference between Node Affinity and Node Label Selector?

117. What is container runtime in Kubernetes?

118. What is Kubernetes QoS?

119. What are requests and limits in Kubernetes?

120. Explain 3 challenges that you faced while working on Kubernetes.

121. Can we use Kubernetes Master for scheduling the Pods?

122. Explain Horizontal vs Vertical Scaling in Kubernetes or in general.

123. What are the different types of Secrets in Kubernetes?

## Section 9: Observability

124. What is the difference between monitoring and observability?

125. How do you emit custom logs and metrics in your application?

126. What kind of metrics do you scrape with Prometheus in your current organization?

127. Have you worked on observability? If yes, explain what you did.

128. What is the difference between logs, metrics, and traces?

129. What is the difference between push- and pull-based monitoring?

130. Which tools have you used to build an observability stack?

131. Users report slowness in the app. Logs don't show errors, and CPU is good. Fix it.

132. How do you trace a request across multiple microservices in a Kubernetes cluster?

133. A Pod crashes randomly with OOMKilled. How do you identify and fix this?

134. You got woken up at 2 AM by false alarms. What's your strategy to reduce noise?

## AWS

135. Explain how you will design a highly available and scalable multi-tier app.

136. What is AWS NAT and when is it used?

137. How do you enable Internet access to an application deployed in a private subnet?

138. Can applications in different subnets of a VPC interact by default? If no, why?

139. Explain NACL vs SG and which one do you use in your organization?

140. EC2 instance terminated unexpectedly. How will you troubleshoot?

141. Lambda function fails randomly. How will you fix the issue?

142. What will you do when AWS RDS storage is full?

143. A developer deleted critical resources like S3, RDS, and EC2. What will you do?

144. Explain a cost optimization activity that you performed in the current organization.

145. Explain a recent challenge that you faced with AWS and how did you solve it?

146. Auto Scaling Group not launching EC2. What can be the issue?

147. Which AWS services do you use in your day-to-day life?

148. Have you used AWS EFS? If yes, what issues did you run into?

149. When will you go for EFS over EBS in realtime?

150. Disable AWS console access to the IAM users.

151. How can AWS Lambda in one AWS account connect to an S3 bucket in another account?

152. What is AWS STS and how does it work?

153. What is a trust policy in AWS and why is it used?

154. How can a Lambda function in AWS Account A interact with DynamoDB in Account B?

155. What are the disadvantages of using EBS volumes in Kubernetes?

156. AWS Secrets Manager vs Parameter Store.

157. Explain your day-to-day activities related to databases.

158. Have you worked with Lambda in AWS? Explain your activities with Lambda.

159. What is the difference between an IAM user and a role?

## Section 11: Azure

160. User reports random downtime in a web app hosted in Azure App Services.

161. How can you schedule a script to run every day on Azure?

162. Cannot SSH or RDP to an Azure VM. How will you fix it?

163. Azure Function failed to execute. How do you debug it?

164. How will you restrict access to a storage account so only VMs in a specific VNet can access it?

165. Your team wants to replicate a production SQL database to staging daily. What's your approach?

166. How do you use tags on resources in Azure? Do you enforce them?

167. How do you create resources on Azure: Bicep, ARM, or Terraform?

168. Your team has access to delete resources. How do you prevent accidental deletion?

169. In an AKS cluster, how will you secure app-to-app communication?

170. Explain how you connect Azure VMs to an on-premises database.

171. How do you ensure data redundancy and performance in a multi-region deployment?

172. A VM-based application in East US is slow for users in Europe. What do you do?

173. Service Principals vs Managed Identities: which one is better?

174. Explain how you used NSG and ASG in realtime.

## Section 12: Python

175. What are some common packages that you use as a DevOps Engineer?

176. Day-to-day task: Tell about a task where you used Python.

177. Write a script to find and print a pattern from a huge log file using Python.

## Section 13: Project Management and SDLC

178. Please walk us through a typical day at your work.

179. Tell us about your DevOps experience.

180. What is your contribution in the team? [For 5+ years candidates]

181. What is your contribution in the team? [For < 5 years candidates]

182. Tell us about your project in your current organization.
