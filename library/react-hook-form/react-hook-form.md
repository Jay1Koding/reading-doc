- isDirty는 form level에서 defaultValues 값과 현재 form의 값을 깊은 비교를 해서 나오는 상태 값이다.
- dirtyFields는 각 input의 상태값이다(사용자로부터 값의 변경이 있었는지를 나타내는 상태 값). input의 값에 대한 상태 값이 아니다.

- [react-hook-form-valid](https://www.daleseo.com/react-hook-form/)

- React Hook Form의 핵심 개념 중 하나는 컴포넌트를 후크에 등록하는(register) 것입니다. 이렇게 하면 양식 유효성 검사와 제출에 모두 해당 값을 사용할 수 있습니다.

- 참고: 각 필드에는 등록 프로세스를 위한 키로 이름이 필요합니다.

- React Hook Form은 외부 UI 컴포넌트 라이브러리와 쉽게 통합할 수 있게 해줍니다. 컴포넌트가 입력의 ref를 노출하지 않는다면, 등록 프로세스를 처리하는 Controller 컴포넌트를 사용해야 합니다.

```javascript
import Select from 'react-select';
import { useForm, Controller } from 'react-hook-form';
import Input from '@material-ui/core/Input';

const App = () => {
  const { control, handleSubmit } = useForm({
    defaultValues: {
      firstName: '',
      select: {},
    },
  });
  const onSubmit = (data) => console.log(data);

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <Controller
        name='firstName'
        control={control}
        render={({ field }) => <Input {...field} />}
      />
      <Controller
        name='select'
        control={control}
        render={({ field }) => (
          <Select
            {...field}
            options={[
              { value: 'chocolate', label: 'Chocolate' },
              { value: 'strawberry', label: 'Strawberry' },
              { value: 'vanilla', label: 'Vanilla' },
            ]}
          />
        )}
      />
      <input type='submit' />
    </form>
  );
};
```

- error handling
  - React Hook Form은 양식의 오류를 표시하는 오류 객체를 제공합니다. 오류의 유형은 주어진 유효성 검사 제약 조건을 반환합니다. 다음 예시는 필수 유효성 검사 규칙을 보여줍니다.

```javascript
import { useForm } from 'react-hook-form';

export default function App() {
  const {
    register,
    formState: { errors },
    handleSubmit,
  } = useForm();
  const onSubmit = (data) => console.log(data);

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <input
        {...register('firstName', { required: true })}
        aria-invalid={errors.firstName ? 'true' : 'false'}
      />
      {errors.firstName?.type === 'required' && (
        <p role='alert'>First name is required</p>
      )}

      <input
        {...register('mail', { required: 'Email Address is required' })}
        aria-invalid={errors.mail ? 'true' : 'false'}
      />
      {errors.mail && <p role='alert'>{errors.mail?.message}</p>}

      <input type='submit' />
    </form>
  );
}
```

- yup, AntD, MUI 같은 외부 컴포넌트랑 작업을 피하기 힘드므로 Controller를 제공 받음
  - Yup은 마치 interface나 type declaration 같은 Obj(Scheme)
  - 이를 useForm 안에서 resolving 하고 Form 에서 Validate

TODO : 사용법만 좀 볼 것
