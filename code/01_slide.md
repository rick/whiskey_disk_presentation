!SLIDE code smallest

	rake tasks
	
	deploy:setup
	- should make changes on the specified domain when a domain is specified
	- should make changes on the local system when no domain is specified
	- should ensure that the parent path for the main repository checkout is present
	
	when a configuration repo is specified
	- should ensure that the parent path for the configuration repository checkout is present

	when no configuration repo is specified
	- should not ensure that the path for the configuration repository checkout is present

	- should check out the main repository

	when a configuration repository is specified
	- should check out the configuration repository

	when no configuration repository is specified
	- should not check out the configuration repository

	- should update the main repository checkout
	
	when a configuration repository is specified
	- should update the configuration repository checkout

	when no configuration repository is specified
	- should update the configuration repository checkout

	when a configuration repository is specified
	- should refresh the configuration

	when no configuration repository is specified
	- should not refresh the configuration
	
	- should run any post setup hooks
	- should flush WhiskeyDisk changes
	
	
	
!SLIDE 

<img src="config_wtf.jpg">

<div style="position: absolute; right: 0px; top: 200px; width: 200px">
<h1>WTF is a config repo?</h1>
</div>


!SLIDE full-page

<img src="config_repo.png">	
	


!SLIDE code smallest

	@@@ruby
	
	namespace :deploy do
	  desc "Perform initial setup for deployment"
	  task :setup do
	    WhiskeyDisk.ensure_main_parent_path_is_present
	    WhiskeyDisk.ensure_config_parent_path_is_present      if WhiskeyDisk.has_config_repo?
	    WhiskeyDisk.checkout_main_repository
	    WhiskeyDisk.checkout_configuration_repository         if WhiskeyDisk.has_config_repo?
	    WhiskeyDisk.update_main_repository_checkout
	    WhiskeyDisk.update_configuration_repository_checkout  if WhiskeyDisk.has_config_repo?
	    WhiskeyDisk.refresh_configuration                     if WhiskeyDisk.has_config_repo?
	    WhiskeyDisk.run_post_setup_hooks
	    WhiskeyDisk.flush
	  end
	
	  desc "Deploy now."
	  task :now do
	    WhiskeyDisk.update_main_repository_checkout
	    WhiskeyDisk.update_configuration_repository_checkout  if WhiskeyDisk.has_config_repo?
	    WhiskeyDisk.refresh_configuration                     if WhiskeyDisk.has_config_repo?
	    WhiskeyDisk.run_post_deploy_hooks
	    WhiskeyDisk.flush
	  end
	end

