themedir = $(pkgdatadir)/theme
dist_theme_DATA = \
	panel.css \
	add_status_header.png \
	add_status_header_hover.png \
	status_header_hover.png \
	avatar_icon.png \
	status_entry_box.png \
	alt_status_header.png \
	alt_status_header_hover.png \
	current_status_header.png \
	avatar_frame.png \
	status_card_background.png \
	retweet.png \
	retweet-hover.png \
	tweet-reply.png \
	tweet-reply-hover.png \
	icon-bg-hover.png \
	location-box-bg.png \
	content-panel-background.png \
	panel-background.png \
	background-gradient.png \
	people.png \
	content-panel-header-grey.png

# Manage mutter-meego's texture cache.

if ENABLE_CACHE
texture_cache = $(DESTDIR)$(pkgdatadir)/nbtk.cache
CACHE_GEN = $(Q:@=@echo '  GEN   '$(texture_cache);)

install-data-hook:
	$(CACHE_GEN)$(NBTK_CREATE_IMAGE_CACHE) $(DESTDIR)$(pkgdatadir)

uninstall-hook:
	rm -f $(texture_cache)
	$(CACHE_GEN)$(NBTK_CREATE_IMAGE_CACHE) $(DESTDIR)$(pkgdatadir)
endif
