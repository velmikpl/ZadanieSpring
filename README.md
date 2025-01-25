Zadanie 1 Spring
Aplikacja jest prostym projektem webowym, który obsługuje dwa różne endpointy:
  Strona główna (/), która zwraca prostą wiadomość tekstową.
  Strona powitalna (/greeting), która wyświetla treść HTML opartą na przesłanych parametrach
  
Główna klasa aplikacji FirstProjectJavaSpringApplication jest punktem wejścia i odpowiada za uruchomienie serwera aplikacji, który domyślnie działa na porcie 8080. Projekt korzysta z silnika szablonów Thymeleaf do renderowania widoków, co umożliwia dynamiczne generowanie treści na podstawie danych przekazanych przez kontroler.

Jednym z najważniejszych elementów aplikacji jest klasa HelloController. Jest to kontroler z adnotacją @Controller, który odpowiada za obsługę żądań HTTP i zwracanie odpowiednich odpowiedzi.

Endpoint główny “/” to endpoint, obsługiwany przez metodę hello() i zwraca prostą wiadomość tekstową:

@GetMapping(value="/")
public String hello() {
   return "Hello Vistula, in my first Spring controller";
}

Po wejściu na adres http://localhost:8080/ pojawi się tekst "Hello Vistula, in my first Spring controller". 
Endpoint /greeting to drugi endpoint. Generuje stronę powitalną. Metoda przyjmuje parametr name przekazywany w adresie (np. /greeting?name=Jan). Jeśli parametr name nie zostanie podany, używana jest wartość domyślna "World":

@GetMapping("/greeting")
//@ResponseBody
public String greeting(@RequestParam(name="name", required = false, defaultValue = "World") String name, Model model) {
   model.addAttribute("name", name);
   return "greeting";
}

Metoda dodaje zmienną name do modelu, który zostanie użyty w szablonie Thymeleaf o nazwie greeting.html. Widok zostanie wyrenderowany dynamicznie z wykorzystaniem przesłanej wartości.

Plik HTML greeting.html znajduje się w katalogu resources/templates. Jest to szablon przetwarzany przez Thymeleaf, co pozwala na dynamiczne generowanie treści:

<!DOCTYPE HTML>
<html xmlns:th="http://www.thymeleaf.org">
<head>
   <title>Getting Started: Serving Web Content</title>
   <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
</head>
<body>
<p th:text="'Hello, ' + ${name} + '!'" />
<p>
   Lorem ipsum dolor sit amet, consectetur adipiscing elit. Maecenas vestibulum dignissim malesuada.
</p>
<img th:src="@{/images/logo.jpg}" width="600" height="600" />
</body>
</html>

Zmienna name jest przekazywana przez kontroler. Jeśli użytkownik odwiedzi adres http://localhost:8080/greeting?name=Jan, zobaczy wiadomość "Hello, Jan!".

Szablon zawiera obraz logo.jpg, który znajduje się w katalogu resources/static/images. Jest on wczytywany za pomocą Thymeleaf:

<img th:src="@{/images/logo.jpg}" width="600" height="600" />

Adnotacja @Controller oznacza klasę jako kontroler MVC (Model-View-Controller). Metody w tej klasie zazwyczaj zwracają nazwy widoków, które są renderowane przez Thymeleaf.

W projekcie metoda greeting() zwraca nazwę widoku greeting, który odpowiada plikowi greeting.html. Silnik Thymeleaf automatycznie znajduje ten plik jeśli znajduje się w folderze templates.

Adnotacja @RestController jest używana w aplikacjach typu REST. Sprawia ona, że metody w kontrolerze domyślnie zwracają tekst zamiast widoków. W tym przypadku silnik Thymeleaf nie będzie szukać widoku greetings.html tylko zwróci czysty tekst.

@ResponseBody to adnotacja, która może być przypisana do konkretnej metody kontrolera oznaczonego jako @Controller. Sprawia, że metoda ma zwrócić dane bezpośrednio w odpowiedzi HTTP, a nie nazwę widoku.
