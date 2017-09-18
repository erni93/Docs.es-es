# <a name="adding-a-new-field"></a><span data-ttu-id="63305-101">Adición de un nuevo campo</span><span class="sxs-lookup"><span data-stu-id="63305-101">Adding a new field</span></span>

<span data-ttu-id="63305-102">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="63305-102">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="63305-103">En este tutorial se agregará un nuevo campo a la tabla `Movies`.</span><span class="sxs-lookup"><span data-stu-id="63305-103">This tutorial will add a new field to the `Movies` table.</span></span> <span data-ttu-id="63305-104">Quitaremos la base de datos y crearemos una nueva cuando cambiemos el esquema (agregar un nuevo campo).</span><span class="sxs-lookup"><span data-stu-id="63305-104">We'll drop the database and create a new one when we change the schema (add a new field).</span></span> <span data-ttu-id="63305-105">Este flujo de trabajo funciona bien al principio de desarrollo si no tenemos que conservar datos de producción.</span><span class="sxs-lookup"><span data-stu-id="63305-105">This workflow works well early in development when we don't have any production data to perserve.</span></span>

<span data-ttu-id="63305-106">Una vez que se haya implementado la aplicación y se tengan datos que se quieran conservar, no se podrá desconectar la base de datos cuando sea necesario cambiar el esquema.</span><span class="sxs-lookup"><span data-stu-id="63305-106">Once your app is deployed and you have data that you need to perserve, you can't drop your DB when you need to change the schema.</span></span> <span data-ttu-id="63305-107">[Migraciones de Entity Framework Code First](http://docs.efproject.net/en/latest/platforms/aspnetcore/new-db.html) permite actualizar el esquema y migrar la base de datos sin perder datos.</span><span class="sxs-lookup"><span data-stu-id="63305-107">Entity Framework [Code First Migrations](http://docs.efproject.net/en/latest/platforms/aspnetcore/new-db.html) allows you to update your schema and migrate the database without losing data.</span></span> <span data-ttu-id="63305-108">Migraciones es una característica muy usada con SQL Server, pero SQLite no admite muchas operaciones de esquema de migración, por lo que solo se pueden realizar migraciones muy sencillas.</span><span class="sxs-lookup"><span data-stu-id="63305-108">Migrations is a popular feature when using SQL Server, but SQLlite does not support many migration schema operations, so only very simply migrations are possible.</span></span> <span data-ttu-id="63305-109">Para más información, vea [SQLite EF Core Database Provider Limitations](https://docs.microsoft.com/ef/core/providers/sqlite/limitations) (Limitaciones del proveedor de base de datos de SQLite EF Core).</span><span class="sxs-lookup"><span data-stu-id="63305-109">See [SQLite Limitations](https://docs.microsoft.com/ef/core/providers/sqlite/limitations) for more information.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="63305-110">Adición de una propiedad de clasificación al modelo Movie</span><span class="sxs-lookup"><span data-stu-id="63305-110">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="63305-111">Abra el archivo *Models/Movie.cs* y agregue una propiedad `Rating`:</span><span class="sxs-lookup"><span data-stu-id="63305-111">Open the *Models/Movie.cs* file and add a `Rating` property:</span></span>

<span data-ttu-id="63305-112">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]</span><span class="sxs-lookup"><span data-stu-id="63305-112">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]</span></span>

<span data-ttu-id="63305-113">Dado que ha agregado un nuevo campo a la clase `Movie`, también debe actualizar la lista blanca de direcciones de enlace para que incluya esta nueva propiedad.</span><span class="sxs-lookup"><span data-stu-id="63305-113">Because you've added a new field to the `Movie` class, you also need to update the binding whitelist so this new property will be included.</span></span> <span data-ttu-id="63305-114">En *MoviesController.cs*, actualice el atributo `[Bind]` para que los métodos de acción `Create` y `Edit` incluyan la propiedad `Rating`:</span><span class="sxs-lookup"><span data-stu-id="63305-114">In *MoviesController.cs*, update the `[Bind]` attribute for both the `Create` and `Edit` action methods to include the `Rating` property:</span></span>

```csharp
[Bind("ID,Title,ReleaseDate,Genre,Price,Rating")]
   ```

<span data-ttu-id="63305-115">También necesita actualizar las plantillas de vista para mostrar, crear y editar la nueva propiedad `Rating` en la vista del explorador.</span><span class="sxs-lookup"><span data-stu-id="63305-115">You also need to update the view templates in order to display, create, and edit the new `Rating` property in the browser view.</span></span>

<span data-ttu-id="63305-116">Edite el archivo */Views/Movies/Index.cshtml* y agregue un campo `Rating`:</span><span class="sxs-lookup"><span data-stu-id="63305-116">Edit the */Views/Movies/Index.cshtml* file and add a `Rating` field:</span></span>

<span data-ttu-id="63305-117">[!code-HTML[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexGenreRating.cshtml?highlight=17,39&range=24-64)]</span><span class="sxs-lookup"><span data-stu-id="63305-117">[!code-HTML[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexGenreRating.cshtml?highlight=17,39&range=24-64)]</span></span>

<span data-ttu-id="63305-118">Actualice */Views/Movies/Create.cshtml* con un campo `Rating`.</span><span class="sxs-lookup"><span data-stu-id="63305-118">Update the */Views/Movies/Create.cshtml* with a `Rating` field.</span></span>

<span data-ttu-id="63305-119">La aplicación no funcionará hasta que la base de datos se actualice para incluir el nuevo campo.</span><span class="sxs-lookup"><span data-stu-id="63305-119">The app won't work until we update the DB to include the new field.</span></span> <span data-ttu-id="63305-120">Si la ejecuta ahora, obtendrá el siguiente `SqliteException`:</span><span class="sxs-lookup"><span data-stu-id="63305-120">If you run it now, you'll get the following `SqliteException`:</span></span>

```
SqliteException: SQLite Error 1: 'no such column: m.Rating'.
```

<span data-ttu-id="63305-121">Este error se muestra porque la clase del modelo Movie actualizada es diferente del esquema de la tabla Movie de la base de datos existente.</span><span class="sxs-lookup"><span data-stu-id="63305-121">You're seeing this error because the updated Movie model class is different than the schema of the Movie table of the existing database.</span></span> <span data-ttu-id="63305-122">(No hay ninguna columna `Rating` en la tabla de la base de datos).</span><span class="sxs-lookup"><span data-stu-id="63305-122">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="63305-123">Este error se puede resolver de varias maneras:</span><span class="sxs-lookup"><span data-stu-id="63305-123">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="63305-124">Desconecte la base de datos y haga que Entity Framework vuelva a crear automáticamente la base de datos según el nuevo esquema de clase de modelo.</span><span class="sxs-lookup"><span data-stu-id="63305-124">Drop the database and have the Entity Framework automatically re-create the database based on the new model class schema.</span></span> <span data-ttu-id="63305-125">Con este enfoque se pierden los datos que tenga en la base de datos, así que no puede hacer esto con una base de datos de producción.</span><span class="sxs-lookup"><span data-stu-id="63305-125">With this approach, you lose existing data in the database — so you can't do this with a production database!</span></span> <span data-ttu-id="63305-126">A menudo, una forma productiva de desarrollar una aplicación consiste en usar un inicializador para propagar una base de datos con datos de prueba.</span><span class="sxs-lookup"><span data-stu-id="63305-126">Using an initializer to automatically seed a database with test data is often a productive way to develop an app.</span></span>

2. <span data-ttu-id="63305-127">Modifique manualmente el esquema de la base de datos existente para que coincida con las clases de modelo.</span><span class="sxs-lookup"><span data-stu-id="63305-127">Manually modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="63305-128">La ventaja de este enfoque es que se conservan los datos.</span><span class="sxs-lookup"><span data-stu-id="63305-128">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="63305-129">Puede realizar este cambio de forma manual o mediante la creación de un script de cambio de base de datos.</span><span class="sxs-lookup"><span data-stu-id="63305-129">You can make this change either manually or by creating a database change script.</span></span>

3. <span data-ttu-id="63305-130">Use Migraciones de Code First para actualizar el esquema de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="63305-130">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="63305-131">Para este tutorial, se desconectará la base de datos y se volverá a crear cuando cambie el esquema.</span><span class="sxs-lookup"><span data-stu-id="63305-131">For this tutorial, we'll drop and re-create the database when the schema changes.</span></span> <span data-ttu-id="63305-132">Para desconectar la base de datos, ejecute este comando desde un terminal:</span><span class="sxs-lookup"><span data-stu-id="63305-132">Run the following command from a terminal to drop the db:</span></span>

`dotnet ef database drop`

<span data-ttu-id="63305-133">Actualice la clase `SeedData` para que proporcione un valor para la nueva columna.</span><span class="sxs-lookup"><span data-stu-id="63305-133">Update the `SeedData` class so that it provides a value for the new column.</span></span> <span data-ttu-id="63305-134">A continuación se muestra un cambio de ejemplo, aunque es conveniente realizarlo con cada `new Movie`.</span><span class="sxs-lookup"><span data-stu-id="63305-134">A sample change is shown below, but you'll want to make this change for each `new Movie`.</span></span>

<span data-ttu-id="63305-135">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/SeedDataRating.cs?name=snippet1&highlight=6)]</span><span class="sxs-lookup"><span data-stu-id="63305-135">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/SeedDataRating.cs?name=snippet1&highlight=6)]</span></span>

<span data-ttu-id="63305-136">Agregue el campo `Rating` a la vista `Edit`, `Details` y `Delete`.</span><span class="sxs-lookup"><span data-stu-id="63305-136">Add the `Rating` field to the `Edit`, `Details`, and `Delete` view.</span></span>

<span data-ttu-id="63305-137">Ejecute la aplicación y compruebe que puede crear, editar o mostrar películas con un campo `Rating`.</span><span class="sxs-lookup"><span data-stu-id="63305-137">Run the app and verify you can create/edit/display movies with a `Rating` field.</span></span> <span data-ttu-id="63305-138">plantillas.</span><span class="sxs-lookup"><span data-stu-id="63305-138">templates.</span></span>