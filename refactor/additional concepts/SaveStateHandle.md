##### When using SaveStateHandle in viewmodel class

imaging you have an id or a search parameter and send it as fragment or activity to ViewModel if the app proccess will kill for any reason and you return app from background you app will crash beause its don't have parameter  for this reason you have to use SaveStateHandle and save your id in there 
