# Tanulos
Szoftverfejlesztes

    public class ThingManagerImpl implements ThingManager {

        public static void main(String[] args) {
            ThingManagerImpl manager = new ThingManagerImpl();
            manager.processThings();
    }

    public static class CountryManager {

            private List<Country> countries;
            
            public CountryManager(){
                countries = Collections.unmodifiableList(new CountryRepository().getAll());
            }
    }

