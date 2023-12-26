# angular-latest
Esqueleto de aplicación Angular actualizado


Versiones:

Angular 17.x

Actualizar Angular CLI:
sudo npm install -g @angular/cli

Migración a Jest:

1. Quitar dependencias karma/jasmine

npm uninstall @types/jasmine jasmine-core karma karma-chrome-launcher karma-coverage karma-jasmine karma-jasmine-html-reporter

2. Añadir dependencias jest

npm install @types/jest jest jest-preset-angular --save-dev

3. Borrar el nodo "test" del angular.json

4. Añadir el fichero setup-jest.ts con el siguiente contenido:
    import 'jest-preset-angular/setup-jest';

5. Ejecutar npx jest --init. Esto creará el fichero jest.config.ts. Durante la instalación elegimos las siguientes opciones:

✔ Would you like to use Jest when running "test" script in "package.json"? … yes
✔ Would you like to use Typescript for the configuration file? … yes
✔ Choose the test environment that will be used for testing › jsdom (browser-like)
✔ Do you want Jest to add coverage reports? … yes
✔ Which provider should be used to instrument code for coverage? › v8
✔ Automatically clear mock calls, instances, contexts and results before every test? … yes

Además descomentamos y cambiamos el valor de estas propiedades:
    - preset: 'jest-preset-angular'
    - setupFilesAfterEnv: ['<rootDir>/setup-jest.ts']

6. Modificar el tsconfig.spec.json dejándolo así:
{
  "extends": "./tsconfig.json",
  "compilerOptions": {
    "outDir": "./out-tsc/spec",
    "types": [
      "jest"
    ],
    "esModuleInterop": true,
    "emitDecoratorMetadata": true
  },
  "include": [
    "src/**/*.spec.ts",
    "src/**/*.d.ts"
  ]
}

7. Adaptar el fichero app.component.spec.ts para que el test sea compatible con standalone

import { ComponentFixture, TestBed } from '@angular/core/testing';
import { provideRouter } from '@angular/router';
import { AppComponent } from './app.component';

describe('AppComponent', () => {
  let component: AppComponent;
  let fixture: ComponentFixture<AppComponent>;

  beforeEach(() => {
    TestBed.configureTestingModule({
      imports: [AppComponent],
      providers: [provideRouter([])],
    });

    fixture = TestBed.createComponent(AppComponent);
    component = fixture.componentInstance;
  });

  it('should create the app', () => {
    expect(component).toBeTruthy();
  });
});

