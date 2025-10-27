
Add the Quality Attributes to this file

| Performance        | Environment: Normal operating load                               |
|--------------------|-----------------------------------------------------|
| Source             | student                                             |
| Stimulus           | User submits a query through text or voice          |
| Artifact           | Query processing pipeline (AI + data fetch)         |
| Response           | System interprets and responds                      |
| Response Measure   | Average response time less than 2 seconds           |

---

| Availability       | Environment: Normal and peak load                                 |
|--------------------|------------------------------------------------------|
| Source             |  student, administrator               
| Stimulus           |  users can access the system at any time                   
| Artifact           |  cloud-deployed assistant service               
| Response           |  System remains operational with minimal downtime       
| Response Measure   |  99.5% uptime per month                  




| Scalability        | Environment: Cloud Environment with auto-scaling                  |
|--------------------|------------------------------------------------------|
| Source             |  Administrator               
| Stimulus           |  Concurrent user count increases sharply   
| Artifact           |  Application server and data-access components
| Response           |  System scales resources horizontally
| Response Measure   |  Sustains 5000 concurrent sessions without compromising performance




| Privacy & Security | Environment: Secure network or remote access                      |
|--------------------|------------------------------------------------------|
| Source             | Student, Lecturer, Administrator                
| Stimulus           | User logs in or requests private data                    
| Artifact           | Authentication, authorization, and data-handling components 
| Response           | Access granted only after SSO validation, data encrypted at all times
| Response Measure   | no unauthorized data exposure                   




| Maintainbility     | Environment: Running production environment                       |
|--------------------|------------------------------------------------------|
| Source             | System maintainer               
| Stimulus           | System maintainer pushes new update                    
| Artifact           | Deployment pipeline and configuration management       
| Response           | System updates can be applied with minimal effort and downtime, easy to extend or fix issues 
| Response Measure   | No service interruption                   




| Usability          | Environment: Any supported device                                 |
|--------------------|------------------------------------------------------|
| Source             | Student                
| Stimulus           | Student interacts through web or voice interface      
| Artifact           | Use interface layer                
| Response           | clear conversational flow, user can complete the task easily          
| Response Measure   | more than 85% successful task completion in usability test 




| Interoperability   | Environment: Integration setup phase                              |
|--------------------|------------------------------------------------------|
| Source             | system maintainer, data source system                
| Stimulus           | system connects to new LMS or calendar system       
| Artifact           | API gateway and adapter modules                
| Response           | communicates using standard APIs, supports multiple external data sources      
| Response Measure   | successful data exchange verified by API test suite  




