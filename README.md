# Consumo de APIs en Angular

Este proyecto es una aplicación de Angular que consume una API para mostrar una lista de usuarios en una tabla con paginación. Utiliza un componente y un servicio para gestionar los datos y el diseño de la interfaz.

## Características

- Consumo de una API pública para obtener una lista de usuarios.
- Presentación de los datos en una tabla paginada.
- Diseño estilizado y responsive, con paginación para mejorar la experiencia de usuario.

## Estructura del Proyecto

La carpeta `components/user-list` contiene los archivos principales para el componente `UserListComponent`:

- **`user-list.component.ts`**: Implementa la lógica de negocio del componente, incluyendo la obtención de datos y la paginación.
- **`user-list.component.html`**: Define la estructura HTML para mostrar la tabla de usuarios y los controles de paginación.
- **`user-list.component.css`**: Proporciona los estilos personalizados para la tabla y los botones de paginación.

## Código Esencial

### `user-list.component.ts`

Este archivo implementa el componente de lista de usuarios, el cual incluye la obtención de datos mediante un servicio y la funcionalidad de paginación.

```typescript
import { Component, inject, OnInit } from '@angular/core';
import { CommonModule } from '@angular/common';
import { HttpClientModule } from '@angular/common/http';
import { UserService } from '../../services/user.service';

@Component({
  selector: 'app-user-list',
  standalone: true,
  templateUrl: './user-list.component.html',
  styleUrls: ['./user-list.component.css'],
  imports: [CommonModule, HttpClientModule],
  providers: [UserService]
})
export class UserListComponent implements OnInit {
  users: any[] = [];
  paginatedUsers: any[] = [];
  currentPage: number = 1;
  itemsPerPage: number = 15;

  private userService = inject(UserService);

  ngOnInit(): void {
    // Obtiene los usuarios al iniciar el componente
    this.userService.getUsers().subscribe(data => {
      this.users = data;
      this.updatePaginatedUsers();
    });
  }

  updatePaginatedUsers() {
    const startIndex = (this.currentPage - 1) * this.itemsPerPage;
    const endIndex = startIndex + this.itemsPerPage;
    this.paginatedUsers = this.users.slice(startIndex, endIndex);
  }

  goToPage(page: number) {
    this.currentPage = page;
    this.updatePaginatedUsers();
  }

  get totalPages(): number {
    return Math.ceil(this.users.length / this.itemsPerPage);
  }
}
```

## Respuestas a las Preguntas del Proyecto

1. **¿Qué hace el método `getUsers` en el servicio?**

   El método `getUsers` envía una solicitud HTTP GET para obtener una lista de usuarios desde la API y devuelve un observable. Este observable está suscrito en el componente, permitiendo cargar los datos y mostrarlos en la tabla.

2. **¿Por qué es necesario importar `HttpClientModule`?**

   `HttpClientModule` es fundamental en Angular para realizar solicitudes HTTP. Permite que el componente se comunique con la API y obtenga los datos de forma asíncrona. Sin este módulo, no sería posible utilizar `HttpClient` para gestionar solicitudes.

3. **¿Qué función cumple el método `ngOnInit` en el componente `UserListComponent`?**

   `ngOnInit` se ejecuta cuando el componente se inicializa, y aquí es donde se llama al método `getUsers` del servicio para cargar los datos de la API. Esto asegura que los datos de los usuarios estarán disponibles tan pronto como el componente esté listo para ser mostrado.

4. **¿Para qué sirve el bucle `*ngFor` en Angular? Explique cómo se utiliza en este ejemplo.**

   `*ngFor` es una directiva que permite iterar sobre una lista de elementos en Angular. En este caso, se utiliza para recorrer `paginatedUsers` y crear dinámicamente una fila (`<tr>`) en la tabla para cada usuario. Cada fila muestra el **ID**, **Nombre**, **Email** y **Rol** de cada usuario.

## Preguntas de Reflexión

1. **¿Qué ventajas tiene el uso de servicios en Angular para el consumo de APIs?**

   Los servicios permiten una separación clara entre la lógica de negocio y la lógica de presentación. Esto facilita la reutilización de código, ya que el servicio puede ser utilizado por múltiples componentes, y mejora el mantenimiento, ya que toda la lógica de manejo de datos está centralizada en un solo lugar.

2. **¿Por qué es importante separar la lógica de negocio de la lógica de presentación?**

   La separación permite que el código sea más fácil de mantener y probar. Los servicios pueden ser testeados de forma independiente y modificados sin afectar directamente la interfaz de usuario. Los componentes, al centrarse solo en la presentación, se vuelven más simples y se pueden reutilizar en distintos contextos.

3. **¿Qué otros tipos de datos o APIs podrían integrarse en un proyecto como este?**

   Además de usuarios, se podrían integrar APIs que proporcionen información de productos, órdenes de compra, estados del clima, noticias, entre otros. Esto dependerá del propósito del proyecto y de los datos necesarios para satisfacer las necesidades de los usuarios.

------------

Ambrocio Sánchez Kevin Slater


