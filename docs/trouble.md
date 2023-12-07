# **TROUBLE SHOOTING IN MIGRATIONS** 
---
## Trouble shooting circular dependency issue 

![Screenshot](/extras/circular_error.png)

-  if you encountered with the above issue , kindly follow the below steps to revoke it .
- make sure if your'e performing those steps with new DB without schemas

### CAUSE OF ISSUE 

1. accounts app dependents on transaction and vice versa 
2. there is no cause such queries can be done as reference models 'accounts.parties'


### STEPS 
---
1. Delete the migrations folder in ***accounts/migrations*** and in ***transaction/migrations***.
2. Navigate to ***accounts/models*** , now you can find the **userprocessauth** model 
3. now comment the section in your IDE 

    === "BEFORE"

        ``` 
        line no : 360  in accounts/models
        ---

        class userprocessauth(models.Model):
            sign_required = models.IntegerField()
            data_entry = models.BooleanField(default=False, blank=True, null=True)
            sign_a = models.BooleanField(default=False, blank=True, null=True)
            sign_b = models.BooleanField(default=False, blank=True, null=True)
            sign_c = models.BooleanField(default=False, blank=True, null=True)

            """ comment the below section including meta class before migrations  """
            """ uncommment once on successful migration and remigrate """

            user = models.ForeignKey(User, on_delete=models.CASCADE)
            model = models.ForeignKey(Flowmodel, on_delete=models.DO_NOTHING)
            action = models.ForeignKey(Action, on_delete=models.DO_NOTHING)

            def __str__(self):
                return f'{self.model.description.split(".")[1].upper()[:-1]} - {self.action} - {self.user}'

            class Meta:
                verbose_name_plural = "User authorization"
                unique_together = ("action", "user", "model")
                indexes = [
                    models.Index(fields=["user"]),
                    models.Index(fields=["model"]),
                    models.Index(fields=["action"]),
                ]
        ```

    === "AFTER"

        ``` 
        class userprocessauth(models.Model):
            sign_required = models.IntegerField()
            data_entry = models.BooleanField(default=False, blank=True, null=True)
            sign_a = models.BooleanField(default=False, blank=True, null=True)
            sign_b = models.BooleanField(default=False, blank=True, null=True)
            sign_c = models.BooleanField(default=False, blank=True, null=True)

        ```


4. Navigate to ***accounts/models*** , now you can find the **Partyaction** model 


    === "BEFORE"

        ``` 
        class PartyAction(Action):

            def __str__(self):
                return self.description

        class Meta:
            proxy = True
            ordering = ['id']
            verbose_name_plural = "Finflo Action"

        @property
        def model_name(self):
            return self.model.description
        ```

    === "AFTER"

        ``` 
        class PartyAction(models.Model): 
            pass
        ```



5. Navigate to **accounts/admin** and comment the below code in **line no. 70**.

        admin.site.register(PartyAction , ActionModelAdmin)
    
6. Now apply the migrations

    
        - python manage.py makemigrations accounts
        - python manage.py makemigrations transaction
        - python manage.py migrate --run-syncdb
    

7. now undo the **point 3** and **4** and **5** redo the **point 6**


<br>

# **DATABASE DELETION AND MIGRATIONS ROLLBACK**

---
### ❗️ NOTE : **REMEMBER THERE IS NO ROLL_BACK ONCE DATA BEEN DELETED**


## CASE 1 :

1. If you need to delete all data from database 

        - python manage.py flush 
        - python manage.py sqlflush 
        - python manage.py dbflush



2. Drop all the tables in the database

        - python manage.py drop_tables


3. Delete the migrations folder found in   **accounts/migration** and **transaction/migrations**.


4. Follow the steps in [troubleshoot](#trouble-shooting-in-migrations) section , if there is any **circular dependency error** .

5. Now migrate the project with 

    
        - python manage.py makemigrations accounts
        - python manage.py makemigrations transaction
        - python manage.py migrate 
        - python manage.py migrate --run-syncdb
    


6. Now again add the FinfloAction proxy model and apply finflo migration


        - python manage.py migrate 
        - python manage.py migrate --fake finflo 0001
        - python manage.py migrate finflo 0002



7. Now create a superuser and run the test cases to make sure all the api are working fine

        - python manage.py createsuperuser
      



## CASE 2 : 

#### ROLL BACK MIGRATIONS 

#### ** rare case
1. Roll back the current migration for a app with 

        - python manage.py migrate appname zero 
        - python manage.py migrate appname 0001_initial
        - python manage.py migrate appname 0002_auto
    



#### NEW MODEL IN FINFLO 

1. Addition of new work model in finflo settings can be acheived via below command 

    
        - python manage.py migrate 
        - python manage.py migrate --fake finflo 0001
        - python manage.py migrate finflo 0002
        - python manage.py migrate --fake accounts
        - python manage.py migrate accounts
        - python manage.py migrate --fake transaction 
        - python manage.py migrate transaction 
    



<br>

# **DATA BACKUP**


#### DUMPING DATA INTO JSON

- dumping all data

        - python manage.py dumpdata > data.json

    
- dumping particular app data

        - python manage.py dumpdata accounts > accounts.json


- dumping particular app model data

        - python manage.py dumpdata accounts.users > users.json