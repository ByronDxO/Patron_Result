# Patrón Result en C#

## Introducción
El **Patrón Result** es una alternativa eficaz al manejo tradicional de errores utilizando `try-catch`, proporcionando una manera más clara y explícita de gestionar las operaciones que pueden fallar. Este patrón encapsula tanto el resultado exitoso como el fallo de una operación, mejorando la legibilidad y mantenibilidad del código.

## ¿Cuándo usar el Patrón Result?
1. **Complejidad con `try-catch`**: Si el uso de bloques `try-catch` hace que el código sea difícil de seguir y mantener.
2. **Frecuencia de Excepciones**: Si las excepciones se lanzan con frecuencia, afectando el rendimiento.
3. **Control de Flujo Claro**: Si se necesita un flujo de control más limpio y predecible.

## Origen del Patrón Result
El patrón Result surge de la necesidad de gestionar errores de manera más controlada y explícita en las aplicaciones, evitando la propagación descontrolada de excepciones y mejorando la robustez y claridad del código.

## Implementación

### Clase Result
La clase Result es una estructura genérica que representa el resultado de una operación, indicando si fue exitosa o no, y encapsulando cualquier dato o error resultante.

```csharp
public class Result
{
    public bool IsSuccess { get; }
    public string Error { get; }

    protected Result(bool isSuccess, string error)
    {
        IsSuccess = isSuccess;
        Error = error;
    }

    public static Result Success() => new Result(true, null);
    public static Result Failure(string error) => new Result(false, error);
}

public class Result<T> : Result
{
    public T Value { get; }

    private Result(T value, bool isSuccess, string error) : base(isSuccess, error)
    {
        Value = value;
    }

    public static Result<T> Success(T value) => new Result<T>(value, true, null);
    public static Result<T> Failure(string error) => new Result<T>(default(T), false, error);
}

```
## Uso del Patrón Result
### Ejemplo de uso en un método que realiza una división:

```csharp
public Result<int> Divide(int numerator, int denominator)
{
    if (denominator == 0)
        return Result<int>.Failure("Denominator cannot be zero.");

    return Result<int>.Success(numerator / denominator);
}

var result = Divide(10, 0);
if (result.IsSuccess)
{
    Console.WriteLine($"Result: {result.Value}");
}
else
{
    Console.WriteLine($"Error: {result.Error}");
}

```

## Ventajas del Patrón Result
* Claridad del Código: Mejora la legibilidad del código, haciendo explícito cuándo una operación puede fallar.
* Manejo de Errores Eficiente: Los errores se gestionan de manera explícita y controlada, reduciendo la necesidad de try-catch.
* Mejora del Flujo de Control: Facilita un flujo de control más limpio y predecible.
## Herramientas y Librerías
* FluentResults: Una librería ligera que implementa el patrón Result de manera extensible y moderna. Proporciona una forma estandarizada y eficiente de manejar resultados y errores en .NET.
### Ejemplo con FluentResults
### Instalación de la librería:
```csharp
Install-Package FluentResults
using FluentResults;

public Result<User> CreateUser(string email)
{
    if (!email.Contains('@'))
    {
        return Result.Fail(new Error("Email must contain '@'"));
    }

    var user = new User { Email = email };
    return Result.Ok(user);
}
```
### Ejemplo del Video
En el video [Una Alternativa para los Try Catch | Result Pattern](https://www.youtube.com/watch?v=jgKvknjsU7Q&t

```
