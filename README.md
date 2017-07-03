```php
  /**
   * Gets the source files list.
   *
   * Grabs files from any directory listed in the sources array in structure
   * configuration.
   *
   * @return void
   */
  public function listSources()
  {
    $app_path        = $this->paths['project']['application'];
    $app_paths       = $this->paths['sources']['app'];
    $app_asset_paths = $this->paths['assets']['app'];
    $app_image_paths = $app_path . DS . $this->paths['assets']['app']['img'];
    $modules_path    = $app_path . DS . $app_paths['modules'];
    $sys_paths       = $this->paths['sources']['system'];
    $sys_asset_paths = $this->paths['assets']['theme'];
    $sys_image_paths = $this->paths['assets']['theme']['img'];
    unset($sys_asset_paths['img']);
    unset($app_asset_paths['img']);
    unset($app_paths['modules']);

    // System Sources.
    foreach ($sys_paths as $src_path) {
      foreach ($this->listFiles($src_path) as $file) {
        $this->file_lists['system'][$file] = $src_path;
      }
    }

    // System Assets.
    foreach ($sys_asset_paths as $src_path) {
      foreach ($this->listFiles($src_path) as $file) {
        $this->asset_lists['system'][$file] = $src_path;
      }
    }

    // System Images.
    foreach ($this->listFiles($sys_image_paths) as $file) {
      $this->image_lists['system'][$file] = $sys_image_paths;
    }

    // Application Sources.
    foreach ($app_paths as $src_path) {
      foreach ($this->listFiles($app_path . DS . $src_path) as $file) {
        $this->file_lists['app'][$file] = $app_path . DS . $src_path;
      }
    }

    // Application Assets.
    foreach ($app_asset_paths as $src_path) {
      foreach ($this->listFiles($app_path . DS . $src_path) as $file) {
        $this->asset_lists['app'][$file] = $app_path . DS . $src_path;
      }
    }

    // Application Images.
    foreach ($this->listFiles($app_image_paths) as $file) {
      $this->image_lists['app'][$file] = $app_path . DS . $app_image_paths;
    }

    // Add Modules Recursively.
    foreach ($this->listFolders($modules_path) as $directory) {
      $this->listModuleSources($modules_path . DS . $directory);
    }

    // Add the header file to start of files list.
    $header = $this->file_lists['system']['app_header.php'];
    unset($this->file_lists['system']['app_header.php']);
    $this->file_lists['system'] = array_merge(['app_header.php' => $header], $this->file_lists['system']);

    // Add the file footer to the end of the list.
    $footer = $this->file_lists['system']['app_footer.php'];
    unset($this->file_lists['system']['app_footer.php']);
    $this->file_lists[]['app_footer.php'] = $footer;
  }
```
