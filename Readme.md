## Implementando Web APIs

Demostración de como desarrollar una Web Api realizada en .NET Core Version="2.1.1"

Para ello creamos el siguiente controlador que nos permite obtener un grupo de personas o una de ellas seleccionada por su id.

~~~

    [Route("api/[controller]")]
    [ApiController]
    public class PersonController : ControllerBase
    {
        private List<Person> _people = new List<Person>();

        public PersonController()
        {
            _people.Add(new Person() { Id = 1, FirstName = "James", LastName = "Sprayberry" });
            _people.Add(new Person() { Id = 2, FirstName = "Jason", LastName = "Mosley" });
            _people.Add(new Person() { Id = 3, FirstName = "Jennifer", LastName = "Dietz" });
            _people.Add(new Person() { Id = 4, FirstName = "Bessie", LastName = "Duppstadt" });
        }

        [HttpGet]
        [Produces("application/xml")]
        public List<Person> GetAll()
        {
            return _people;
        }

        [HttpGet("{id}")]
        public ActionResult<Person> GetPersonById(int id)
        {
            var person = _people.FirstOrDefault(p => p.Id == id);

            if (person == null)
            {
                return NotFound();
            }

            return person;
        }
    }

~~~

La información se muestra se devuelve en formato JSON.

Para poder mostrarlo en formato XML, instalamos el siguiente paquete nuget Microsoft.AspNetCore.Mvc.Formatters.Xml y añadimos 
la siguiente líena en el método del controlador

        [HttpGet]
        [Produces("application/xml")] 
        public List<Person> GetAll()
        {

Y en el middleware realizamos la siguiente inyecciónde dependencia.

        public void ConfigureServices(IServiceCollection services)
        {
            services.AddMvc().AddXmlSerializerFormatters();
        }       
