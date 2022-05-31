Author: NieYuhua,

Email: nieyuhua0115@foxmail.com

Current Affiliation: UESTC

Future Affiliation:  University of Washington, Seattle

supervisor: Jin Qi

Date: 5/31/2022

note: this work was done while I pursued my bachelor degree in Jin Qi's AIML Lab in UESTC

# streamlit-fastapi-model-serving

Simple example of usage of streamlit and FastAPI for ML model serving described on [this blogpost](https://davidefiocco.github.io/streamlit-fastapi-ml-serving) and [PyConES 2020 video](https://www.youtube.com/watch?v=IvHCxycjeR0).

When developing simple APIs that serve machine learning models, it can be useful to have _both_ a backend (with API documentation) for other applications to call and a frontend for users to experiment with the functionality.

In this example, we serve an [image semantic segmentation model](https://pytorch.org/hub/pytorch_vision_deeplabv3_resnet101/) using `FastAPI` for the backend service and `streamlit` for the frontend service. `docker-compose` orchestrates the two services and allows communication between them.

To run the example in a machine running Docker and docker-compose, run:

    docker-compose build
    docker-compose up

in the first time, above step would download model from (https://pytorch.org/hub/pytorch_vision_deeplabv3_resnet101/). to avoid to download the model from potential slow website pytorch.org in china, you can also download model deeplabv3_resnet101_coco-586e9e4e.pth from baidu: https://pan.baidu.com/s/1f7-m6E7Z12ABaHSDexZfoA?pwd=5n3i. then put the model in the directory ./ImageSegmentation/Deployment/fastapi/model/. then run:

    docker-compose build
    docker-compose up

To visit the FastAPI documentation of the resulting service, visit http://localhost:8000 with a web browser.  
To visit the streamlit UI, visit http://localhost:8501.

Logs can be inspected via:

    docker-compose logs

### Deployment

To deploy the app, one option is deployment on Heroku (with [Dockhero](https://elements.heroku.com/addons/dockhero)). To do so:

- rename `docker-compose.yml` to `dockhero-compose.yml`
- create an app (we refer to its name as `<my-app>`) on a Heroku account
- install locally the Heroku CLI, and enable the Dockhero plugin with `heroku plugins:install dockhero`
- add to the app the DockHero add-on (and with a plan allowing enough RAM to run the model!)
- in a command line enter `heroku dh:compose up -d --app <my-app>` to deploy the app
- to find the address of the app on the web, enter `heroku dh:open --app <my-app>`
- to visualize the api, visit the address adding port `8000/docs`, e.g. `http://dockhero-<named-assigned-to-my-app>-12345.dockhero.io:8000/docs`(not `https`)
- visit the address adding `:8501` to visit the streamlit interface
- logs are accessible via `heroku logs -p dockhero --app <my-app>`

### Debugging

To modify and debug the app, [development in containers](https://davidefiocco.github.io/debugging-containers-with-vs-code) can be useful (and kind of fun!).
