const { control, reset, setValue } = useForm({
    defaultValues: {
        [formName]: Array(count).fill({ label: 'Application Type', name: 'Application' })
    },
    formName
});

const { fields, append, remove } = useFieldArray({
    control,
    name: formName
});

// Effect to synchronize the number of rows with the `count`
useEffect(() => {
    // Clear existing fields
    if (fields.length > count) {
        // Remove extra fields if count decreased
        for (let i = fields.length; i > count; i--) {
            remove(i - 1);
        }
    } else if (fields.length < count) {
        // Append new fields if count increased
        for (let i = fields.length; i < count; i++) {
            append({ label: 'Application Type', name: 'Application' });
        }
    }
}, [count, fields.length, append, remove]);

// Optionally, reset the form to the new default values whenever `count` changes
useEffect(() => {
    reset({
        [formName]: Array(count).fill({ label: 'Application Type', name: 'Application' })
    });
}, [count, reset]);
