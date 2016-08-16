---
layout: post
title:  "JSON String to Java object using GSON"
date:   2016-08-16 00:00:00 +0900
categories: jekyll update
type: Android
excerpt_separator: <!--more-->
---
<!--more-->
As the title said, here we want to convert a JSON string to a Java model by using GSON library. Basically we have few steps below to complete this process.

Steps
---

#1. Create project and add library to it

New a Java project, add the GSON libary to the Gradle file like this:

```
dependencies {
    compile 'com.google.code.gson:gson:2.7'
}
```

#2. Create the data model

Followed with the structure of JSON sting like this,

```
{
  "data":
    {
      "jan_cd": string,
      "product_name": string,
    },
  "id": string
}
```

and create the corresponding data model.

``` java
public class DataWrapper {

    private static final String JSON_JANCODE= "jan_code";
    private static final String JSON_PRODUCT_NAME = "product_name";


    public static class Data {
        private String jancode;
        private String product_name;
        private String id;

        public Data(String jancode, String product_name, String id) {
            this.jancode = jancode;
            this.product_name = product_name;
            this.id = id;
        }

        public final String getJancode() {
            return jancode;
        }

        public final String getProduct_name() {
            return product_name;
        }

        public final String getId() {
            return id;
        }
    }

    public static class DataDeserializer implements JsonDeserializer<Data> {

        @Override
        public Data deserialize(JsonElement json, Type typeOfT, JsonDeserializationContext context) throws JsonParseException {


            JsonObject jobject = json.getAsJsonObject();

            if (jobject.isJsonNull()) {
                throw new AssertionError("Object cannot be null");
            }

            // json member 1: data
            JsonObject jobject_data = jobject.getAsJsonObject("data");
            String jan_code         = jobject_data.get(JSON_JANCODE).getAsString();
            String product_name     = jobject_data.get(JSON_PRODUCT_NAME).getAsString();

            // json member 2: id
            String id = jobject.get("id").getAsString();

            DataWrapper.Data data = new Data(jan_code, product_name, id);

            return data;
        }
    }
}

```

#3. Write a converter

Test the conversion like this:

``` java
// convert json string to model
String json = new String("INPUT THE JSON STRING YOU GOT");

// custom deserialize
Gson gson = new GsonBuilder().registerTypeAdapter(DataWrapper.Data.class, new DataWrapper.DataDeserializer()).create();
Type type = new TypeToken<DataWrapper.Data>(){}.getType();
DataWrapper.Data datas = gson.fromJson(json, type);
```

[Returned JsonObject Null for only some GSON method calls][R2]
---
You may meet an error like getting a null object after deserialization/conversion. The error message may be like:

```
java.lang.NullPointerException: Attempt to invoke virtual method 'java.lang.String com.google.gson.JsonElement.toString()' on a null object reference
```

Check your JSON structure carefully. For example, if you have this:

```
{
    "data": {
        "id": "lDRB2",
        "title": "Imgur Office",
        "description": null,
        "datetime": 1357856292,
        "cover": "24nLu",
        "account_url": "Alan",
        "account_id": 4,
        "privacy": "public",
        "layout": "blog",
        "views": 13780,
        "link": "http://alanbox.imgur.com/a/lDRB2",
        "images_count": 11,
        "images": [
            {
                "id": "24nLu",
                "title": null,
                "description": null,
                "datetime": 1357856352,
                "type": "image/jpeg",
                "animated": false,
                "width": 2592,
                "height": 1944,
                "size": 855658,
                "views": 135772,
                "bandwidth": 116174397976,
                "link": "http://i.imgur.com/24nLu.jpg"
            },
            {
                "id": "Ziz25",
                "title": null,
                "description": null,
                "datetime": 1357856394,
                "type": "image/jpeg",
                "animated": false,
                "width": 2592,
                "height": 1944,
                "size": 919391,
                "views": 135493,
                "bandwidth": 124571044763,
                "link": "http://i.imgur.com/Ziz25.jpg"
            },
            {
                "id": "9tzW6",
                "title": null,
                "description": null,
                "datetime": 1357856385,
                "type": "image/jpeg",
                "animated": false,
                "width": 2592,
                "height": 1944,
                "size": 655028,
                "views": 135063,
                "bandwidth": 88470046764,
                "link": "http://i.imgur.com/9tzW6.jpg"
            }
        ]
    },
    "success": true,
    "status": 200
}
```

You should respectively use

``` java
JsonObject jobject = json.getAsJsonObje‌​ct();
JsonObject album = jobject.getAsJsonObje‌​ct("data");
JsonArray images = album.getAsJsonArray‌​("images");
boolean success = jobject.get("success").getAsBoolean();
```
to access inner elements.


Reference
---
[Google Gson for converting Java objects to JSON and JSON to Java with - Tutorial][R1]<br />
[Gson User Guide - Serializing and Deserializing Collection with Objects of Arbitrary Types][R3]<br />
[How do I write a custom JSON deserializer for Gson?][R4]<br />

[R1]: http://www.vogella.com/tutorials/JavaLibrary-Gson/article.html#exercise-exclude-a-field-from-a-java-object
[R2]: http://stackoverflow.com/questions/29181972/returned-jsonobject-null-for-only-some-gson-method-calls
[R3]: https://sites.google.com/site/gson/gson-user-guide
[R4]: http://stackoverflow.com/questions/6096940/how-do-i-write-a-custom-json-deserializer-for-gson

