## **OpenMRS ↔ OpenELIS Integration (Step-by-Step)**

### **1)** Setup `openmrs-distro-his` project

- [Clone](https://github.com/openmrs/openmrs-distro-his) the project
- Add `docker-compose-openelis.yml` to `/scripts/docker-compose-files.txt`
- Navigate to `pom.xml` file in the project
- Follow the instruction by (un)commenting the lines of code specified in `pom.xml`

```xml
<!-- Deletes labonfhir globalproperties depending on place of deployment -->
<execution>
    <id>Delete labonfhir globalproperties depending on place of deployment</id>
    <phase>package</phase>
    <configuration>
        <target>
            <!-- Uncomment this line when you want to run OpenELIS Global locally -->
            <!--<delete file="${project.build.directory}/${project.artifactId}-${project.version}/distro/configs/openmrs/initializer_config/globalproperties/03_distro-his/labonfhir-dev-server.xml"/>-->
            <!-- Comment this line when you want to run OpenELIS Global locally -->
            <delete file="${project.build.directory}/${project.artifactId}-${project.version}/distro/configs/openmrs/initializer_config/globalproperties/03_distro-his/labonfhir-local.xml"/>
        </target>
    </configuration>
    <goals>
        <goal>run</goal>
    </goals>
</execution>
```

This ensures that the right configuration is loaded for OpenELIS when running locally.

- Now **run Ozone** follow the instruction specified [here](https://docs.ozone-his.com/implementers/start-stop/)

### **2) Configure OpenELIS**

### **A. Enable sync from external sources**

- Log in to OpenELIS (`<host>:81`) with `admin:adminADMIN!`.
- Open the legacy admin page: `<host>:81/api/OpenELIS-Global/MasterListsPage`.
- From the left menu, click **Order Entry Configuration**.
- Set **external orders** to **true**.

### **B. Configure an example lab test by adding LOINC mapping**

- Open the legacy admin page: `<host>:81/api/OpenELIS-Global/MasterListsPage`.
- From the left menu, click **Test Management** → **Manage tests**.
- Open a lab test (e.g. **White Blood Cells Count (WBC) (Whole Blood)**).
- Scroll to the top and enter (eg. `53225-9` in the **LOINC** field)
- Repeat for all relevant lab tests.

> At this point, OpenELIS is ready to accept orders from OpenMRS.
>

### **3) Sync an order from OpenMRS to OpenELIS and get results back**

### **A. Place an order in OpenMRS**

- Log in to OpenMRS (`<host>:80/openmrs/spa`) with `jdoe:password`.
- Create a new patient or open an existing patient.
- Start a visit.
- Order a lab test that has a LOINC mapping in OpenELIS (e.g., **White blood cells**).
- Save the order.

### **B. Receive the order in OpenELIS**

- In OpenELIS, go to **Incoming Orders**: `<host>:81/ElectronicOrders`.
- Wait ~10 seconds for the scheduler to pick up the order from OpenMRS.
- Click **Search**.
- You should see a new order with the patient’s name.
- Click **Enter Order**.
- Most fields will be auto-filled; proceed through the screens and click **Submit**.

### **C. Enter and validate results in OpenELIS**

- Return to the OpenELIS home screen and click the **In Progress** tile.
- Enter and save the test results.
- Go back to the home screen and click **Ready For Validation**.
- Verify and save the results.
- Back on the home screen, the **Orders Completed Today** tile should show +**1** (or higher if other orders were already completed).

### **D. View results in OpenMRS**

- Open the Patient chart in OpenMRS and click **Results** page on the left.
- You should now see the test results synced back to OpenMRS.