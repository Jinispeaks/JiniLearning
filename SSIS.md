                                        # Learning SSIS 
What is SSIS configuration?
      Configuration are used to make packages _dynamic_ so that it can be deployed from one evironment to another.
      It contains connection manger details like serer details,variables and folder details.
Diff Methods
        i) xml file
        ii) sqlserver table ( right click on control flow go to pkg config and add details)--> dbo.SSISConfigurations

        Environment Variables - these variables hold value which env .Each time the package runs it reads the env variable and point to the required Sql server table or XML.
        SSIS Catalog repoistory 

      
