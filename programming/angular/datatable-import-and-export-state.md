# DataTable import and export state

```typescript
       {
          extend: 'colvis',
          text: 'Column Visibility',
          className: 'buttons-colvis',
          columns: ':not(.noVis)' // Exclude columns with the class 'noVis'
        },
        
          
        {
          text: 'Export State',
          className: 'btn-export-state',
          action: () => {
            this.exportTableState();
          }
        },
        {
          text: 'Import State',
          className: 'btn-import-state',
          action: () => {
            this.triggerImportFileDialog();
          }
        },
        
        
      ...(customProps?.extraButtons || [])  // Allow additional buttons to be added dynamically
      ],

    };
  };

```

```typescript
 /**
   * Export the current table state to a JSON file
   */
  exportTableState(): void {
    try {
      const component = this.getCurrentComponent();
      const component_name = component ? component.name.toLowerCase() + '_table_state' : 'default_table_state';
      const storedState = localStorage.getItem(component_name);

      if (!storedState) {
        this.toastr.warning('No table state found to export.', 'Warning');
        return;
      }

      const state = JSON.parse(storedState);
      const dataStr = JSON.stringify(state, null, 2);
      const dataBlob = new Blob([dataStr], { type: 'application/json' });
      const url = URL.createObjectURL(dataBlob);
      const link = document.createElement('a');
      link.href = url;
      link.download = `${component_name}_${new Date().getTime()}.json`;
      document.body.appendChild(link);
      link.click();
      document.body.removeChild(link);
      URL.revokeObjectURL(url);

      this.toastr.success('Table state exported successfully.', 'Success');
    } catch (error) {
      console.error('Error exporting table state:', error);
      this.toastr.error('Failed to export table state.', 'Error');
    }
  }

  /**
   * Import table state from a JSON file
   * @param file - The JSON file containing the table state
   */
  importTableState(file: File): void {
    try {
      const reader = new FileReader();
      reader.onload = (e: any) => {
        try {
          const importedState = JSON.parse(e.target.result);
          const component = this.getCurrentComponent();
          const component_name = component ? component.name.toLowerCase() + '_table_state' : 'default_table_state';

          // Validate the imported state is an object
          if (!importedState || typeof importedState !== 'object') {
            throw new Error('Invalid state format. Expected a JSON object.');
          }

          // Save the imported state to localStorage
          localStorage.setItem(component_name, JSON.stringify(importedState));
          this.toastr.success('Table state imported successfully. Please refresh the page to apply changes.', 'Success');
        } catch (parseError) {
          console.error('Error parsing imported state:', parseError);
          this.toastr.error('Invalid JSON file format.', 'Error');
        }
      };

      reader.onerror = () => {
        console.error('Error reading file');
        this.toastr.error('Failed to read the file.', 'Error');
      };

      reader.readAsText(file);
    } catch (error) {
      console.error('Error importing table state:', error);
      this.toastr.error('Failed to import table state.', 'Error');
    }
  }

  /**
   * Clear all table states
   */
  clearTableState(): void {
    try {
      const component = this.getCurrentComponent();
      const component_name = component ? component.name.toLowerCase() + '_table_state' : 'default_table_state';
      localStorage.removeItem(component_name);
      this.toastr.success('Table state cleared successfully.', 'Success');
    } catch (error) {
      console.error('Error clearing table state:', error);
      this.toastr.error('Failed to clear table state.', 'Error');
    }
  }

  /**
   * Trigger file input dialog for importing table state
   */
  private triggerImportFileDialog(): void {
    const input = document.createElement('input');
    input.type = 'file';
    input.accept = '.json';
    input.onchange = (event: any) => {
      const file = event.target.files[0];
      if (file) {
        this.importTableState(file);
      }
    };
    input.click();
  }
```
